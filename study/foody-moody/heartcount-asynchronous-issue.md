---
description: 좋아요 개수 비동기 문제
---

# HeartCount Asynchronous Issue

## 상황 및 문제

댓글 좋아요 개수 기능을 구현하게 되였는데 비동기로 댓글 하트 카운트 증가하는데 문제가 발생했습니다. IncorrectResultSizeDataAccessException 및 NonUniqueResultException 발생했습니다.

### 코드

```java
@Transactional
public void increment(CommentId commentId) {
    Optional<CommentHeartCount> heartCount = repository.findByCommentId(commentId);
    if (heartCount.isPresent()) {
        heartCount.get().increment();
    } else {
        repository.save(new CommentHeartCount(CommentHeartCountIdFactory.newId(), commentId, START_INDEX));
    }
}
```

### 테스트 코드

```java
@DisplayName("댓글 하트 카운트를 비동기로 증가시키면 증가시키는 개수만큼 증가한다")
@Test
void increment() throws InterruptedException {
    // given
    CommentId commentId = new CommentId("1");
    CommentHeartCountId commentHeartCountId = CommentHeartCountIdFactory.newId();
    commentHeartCountJpaRepository.save(new CommentHeartCount(commentHeartCountId, commentId, 1L));
    CountDownLatch latch = new CountDownLatch(100);
    AtomicInteger count = new AtomicInteger(1);

    // when
    for (int i = 0; i < 100; i++) {
        threadPoolExecutor.execute(() -> {
            latch.countDown();
            commentHeartCountWriteService.increment(commentId);
            count.incrementAndGet();
        });
    }


    threadPoolExecutor.awaitTermination(1000, TimeUnit.MILLISECONDS);

    // then
    Optional<CommentHeartCount> heartCount = commentHeartCountJpaRepository.findByCommentId(commentId);
    assertThat(heartCount).isPresent();
    assertThat(heartCount.get().getCount()).isEqualTo(count.get());
}
```

## 해결 방법

### **JPA 락 (Java Persistence API Lock)**

#### **낙관적 락 (Optimistic Lock)**:&#x20;

* 데이터 충돌을 **뒤늦게 감지**하는 방식으로, 주로 `@Version` 애너테이션을 사용하여 엔티티에 버전 **번호**나 **타임스탬프**를 부여함으로써 구현됩니다.
* 데이터가 동시에 수정되었을 때 `ObjectOptimisticLockingFailureException` 예외가 발생할 수 있습니다. 이 예외는 다른 트랜잭션이 이미 데이터를 변경했을 때 발생하며, 이를 통해 데이터 일관성을 유지할 수 있습니다.
* 코드 예시:

<pre class="language-java"><code class="lang-java"><strong>@Entity
</strong>public class CommentHeartCount {
    ...

    @Version
    private int version;

    private int count; // '좋아요' 개수
    ...
}

// 증가 로직
@Transactional
public void increment(CommentId commentId) {
    CommentHeartCount heartCount = repository.findByCommentId(commentId)
        .orElseThrow(() -> new IllegalArgumentException("해당하는 댓글이 없습니다."));
    heartCount.increment();
}
</code></pre>

```sql
update
    comment_heart_count 
set
    comment_id=?,
    count=?,
    version=? 
where
    id=? 
    and version=?
```

#### **비관적 락 (Pessimistic Lock)**:&#x20;

* 데이터 충돌을 예방하기 위해 **미리 락**을 설정하는 방식입니다.
* 이 방식은 데이터베이스 수준의 락을 활용하여 **특정 데이터**에 대한 **접근을 차단**합니다.
* 비관적 락은 데이터의 **동시 수정**을 방지함으로써 충돌을 미연에 방지하나, 이로 인해 시스템의 **속도에 영향**을 줄 수 있습니다. 특히, 데이터베이스에 **높은 동시 요청**이 있을 때, 이러한 락은 **성능 저하**를 초래할 수 있습니다.
* 코드 예시:

```java
private final EntityManager entityManager;

@Override
public void incrementCount(CommentId commentId) {
        CommentHeartCount commentHeartCount = entityManager.createQuery(
                        "SELECT c FROM CommentHeartCount c WHERE c.commentId = :commentId",
                        CommentHeartCount.class)
                .setParameter("commentId", commentId)
                .setLockMode(LockModeType.PESSIMISTIC_WRITE)
                .getSingleResult();
        commentHeartCount.increment();
}
```

```sql
select
    commenthea0_.id as id1_3_,
    commenthea0_.comment_id as comment_2_3_,
    commenthea0_.count as count3_3_ 
from
    comment_heart_count commenthea0_ 
where
    commenthea0_.comment_id=? for update
```

### **@Transactional (스프링의 트랜잭션 관리)**:

* `@Transactional` 애너테이션을 사용하여 메서드 또는 클래스 레벨에서 **트랜잭션 경계**를 정의하여 동시성 문제를 해결합니다.
* 동시성을 보장하는데 동시에 같은 데이터에 접근하려 할 때 **Deadlock** 이 발생할 수 있습니다.
* 코드 예시:

```java
import org.springframework.transaction.annotation.Transactional;

public class ProductService {

    @Transactional(isolation = Isolation.SERIALIZABLE)
    public void increment(CommentId commentId) {
        CommentHeartCount heartCount = repository.findByCommentId(commentId)
                .orElseThrow(() -> new IllegalArgumentException("해당하는 댓글이 없습니다."));
        heartCount.increment();
    }
}
```

### 데이터를 먼저 조회하지 않고, 직접 업데이트 쿼리를 실행

* `@Modifying`과 `@Query` 어노테이션을 사용하여 데이터를 먼저 **조회하지 않고** 직접 업데이트 쿼리를 실행합니다
* MySQL의 기본 격리 수준인 `REPEATABLE READ`는 동일한 트랜잭션 내에서 조회한 데이터의 **일관성**을 보장합니다. 동시에 여러 트랜잭션이 **같은 데이터**를 '**쓰기**' 작업을 수행할 때, MySQL은 **내부적**으로 **필요한 락**을 관리하여 데이터 **무결성**을 유지합니다.

```java
@Modifying
@Query("update CommentHeartCount c set c.count = c.count + 1 where c.commentId = :commentId")
void incrementCount(CommentId commentId);
```

## 나의 선택

이 중에서 데이터를 먼저 조회하지 않고 직접 업데이트 쿼리를 실행하는 방식을 선택했습니다. 이 선택의 이유는 다음과 같습니다.

* 트랜잭션 범위를 최소화함으로써 성능 향상을 기대할수 있습니다.
* 락의 범위를 줄임으로써 시스템의 복잡성을 감소시키고, 동시성 문제를 효과적으로 관리할 수 있습니다.
