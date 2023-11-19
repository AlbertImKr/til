---
description: N+1 문제
---

# N+1 Issue

## 상황

### Member Class

```java
@Entity
@Table(name = "member")
public class Member {
    ...
    @Embedded
    private Favorites favorites;
    ...
}
```

### Favorites Class

```java
@Embeddable
public class Favorites {
    ...

    @OneToMany(mappedBy = "member", cascade = {CascadeType.PERSIST, CascadeType.MERGE}, orphanRemoval = true)
    private List<Favorite> favoriteCollection;

    ...
}
```

### FavoriteData Class

```java
@Table(name = "favorite")
@Entity
public class FavoriteDataResponse {

    ... 
    
    @JsonIgnore
    @ManyToOne(cascade = CascadeType.PERSIST)
    @JoinColumn(name = "member_id")
    private Member member;
    @ManyToOne
    @JoinColumn(name = "source_id")
    private StationResponse source;
    @ManyToOne
    @JoinColumn(name = "target_id")
    private StationResponse target;
    
    ...
```

### StationData Class

```java
@Entity
@Table(name = "station")
public class StationData {

    ...
}
```

### FavoriteDataRepository Class

```java
public interface FavoriteDataRepository extends JpaRepository<FavoriteData, Long> {

    List<FavoriteData> findAllByMember(Member member);
}
```

## 문제

### 테스트 결과

#### 하위 Station 만큼 퀴리 N개  및 favoritedate 테이블 조회  퀴리 1개 총 N+1 개 발생한다.

```sql
------------ 즐겨 찾기 전체 조회 요청 ------------
....
Hibernate: 
    select
        favoriteda0_.id as id1_0_,
        favoriteda0_.member_id as member_i2_0_,
        favoriteda0_.source_id as source_i3_0_,
        favoriteda0_.target_id as target_i4_0_ 
    from
        favorite favoriteda0_ 
    where
        favoriteda0_.member_id=?
Hibernate: 
    select
        stationres0_.id as id1_4_0_,
        stationres0_.name as name2_4_0_ 
    from
        station stationres0_ 
    where
        stationres0_.id=?
Hibernate: 
    select
        stationres0_.id as id1_4_0_,
        stationres0_.name as name2_4_0_ 
    from
        station stationres0_ 
    where
        stationres0_.id=?
Hibernate: 
    select
        stationres0_.id as id1_4_0_,
        stationres0_.name as name2_4_0_ 
    from
        station stationres0_ 
    where
        stationres0_.id=?
------------ 즐겨 찾기 전체 조회 요청 ------------
```

## 해결 방법

### 방법1:  JPQL

#### 조인을 사용해서 조회시 station 까지 가져온다

```java
public interface FavoriteDataRepository extends JpaRepository<FavoriteData, Long> {

    @Query("select favorite,source,target,member_T "
            + "from FavoriteData favorite "
            + "join StationData source "
            + "on favorite.source.id = source.id "
            + "join StationData target "
            + "on favorite.target.id = target.id "
            + "join Member member_T "
            + "on favorite.member.id = member_T.id "
            + "where favorite.member = :member")
    List<FavoriteData> findAllByMember(Member member);
}
```

#### 테스트 결과&#x20;

* 하나의 쿼리 문제 해결

```sql
------------ 즐겨 찾기 전체 조회 요청 ------------
Hibernate: 
    select
        favoriteda0_.id as id1_0_0_,
        stationdat1_.id as id1_4_1_,
        stationdat2_.id as id1_4_2_,
        member3_.id as id1_2_3_,
        favoriteda0_.member_id as member_i2_0_0_,
        favoriteda0_.source_id as source_i3_0_0_,
        favoriteda0_.target_id as target_i4_0_0_,
        stationdat1_.name as name2_4_1_,
        stationdat2_.name as name2_4_2_,
        member3_.age as age2_2_3_,
        member3_.email as email3_2_3_,
        member3_.password as password4_2_3_,
        member3_.role as role5_2_3_ 
    from
        favorite favoriteda0_ 
    inner join
        station stationdat1_ 
            on (
                favoriteda0_.source_id=stationdat1_.id
            ) 
    inner join
        station stationdat2_ 
            on (
                favoriteda0_.target_id=stationdat2_.id
            ) 
    inner join
        member member3_ 
            on (
                favoriteda0_.member_id=member3_.id
            ) 
    where
        favoriteda0_.member_id=?
------------ 즐겨 찾기 전체 조회 요청 ------------
```

### 방법2:  EntityGraph

#### EntityGraph의 attributePaths를 사용해서 한번 가져온다

```java
public interface FavoriteDataRepository extends JpaRepository<FavoriteData, Long> {

    @EntityGraph(
        attributePaths = {"source","target","member"}
    )
    List<FavoriteData> findAllByMember(Member member);
}
```

#### 테스트 결과

```sql
------------ 즐겨 찾기 전체 조회 요청 ------------
Hibernate: 
    select
        favoriteda0_.id as id1_0_0_,
        member1_.id as id1_2_1_,
        stationdat2_.id as id1_4_2_,
        stationdat3_.id as id1_4_3_,
        favoriteda0_.member_id as member_i2_0_0_,
        favoriteda0_.source_id as source_i3_0_0_,
        favoriteda0_.target_id as target_i4_0_0_,
        member1_.age as age2_2_1_,
        member1_.email as email3_2_1_,
        member1_.password as password4_2_1_,
        member1_.role as role5_2_1_,
        stationdat2_.name as name2_4_2_,
        stationdat3_.name as name2_4_3_ 
    from
        favorite favoriteda0_ 
    left outer join
        member member1_ 
            on favoriteda0_.member_id=member1_.id 
    left outer join
        station stationdat2_ 
            on favoriteda0_.target_id=stationdat2_.id 
    left outer join
        station stationdat3_ 
            on favoriteda0_.source_id=stationdat3_.id 
    where
        favoriteda0_.member_id=?
------------ 즐겨 찾기 전체 조회 요청 ------------
```

## 장단점

### JPQL 방식

**장점**

1. **유연성**: JPQL을 사용하면 쿼리를 매우 세밀하게 제어할 수 있습니다. 복잡한 조인, 조건, 서브쿼리 등을 사용자 정의로 구현할 수 있습니다.
2. **명시적인 쿼리**: JPQL은 SQL과 유사하게 작성되므로 SQL에 익숙한 개발자에게는 쉽게 읽히고 이해됩니다.

**단점**

1. **복잡성**: 복잡한 쿼리를 작성하면 코드가 길어지고 이해하기 어려워질 수 있습니다.
2. **오류 가능성**: 쿼리에 오타나 로직 오류가 있을 경우, 오류를 찾기 어려울 수 있습니다

### EntityGraph 방식

**장점**

1. **간결함**: EntityGraph를 사용하면 관련 엔티티를 명시적으로 지정하기만 하면 되므로 쿼리가 훨씬 간결해집니다.
2. **오류 감소**: 쿼리의 복잡성이 줄어들면서 잘못된 쿼리 작성에 따른 오류 가능성도 낮아집니다.
3. **유지보수 용이**: 쿼리의 간결함은 유지보수 시 이해하기 쉽고 수정하기 용이하게 만듭니다.

**단점**

1. **유연성 제한**: EntityGraph는 간결하지만, JPQL만큼 세밀한 쿼리 제어가 어렵습니다.
2. **성능 이슈**: 때때로 EntityGraph는 필요 이상의 데이터를 가져올 수 있으며, 이는 성능에 부정적인 영향을 미칠 수 있습니다.

### 결론

* **JPQL**: 복잡하고 세밀한 쿼리가 필요할 때 유용하나, 복잡성과 오류 가능성을 고려해야 합니다.
* **EntityGraph**: 간단하고 명확한 쿼리에 적합하며, 유지보수가 쉽지만, 유연성이 다소 제한됩니다.

## 나의 선택

쿼리의 세밀한 제어와 명식적인 쿼리를 더 좋아하고 복잡성은 감수하고 오류 가능성은 테스트로 충분히 해결 할수 있어서 JPQL를 선택했습니다.

## 기타

#### 멤버 조회시 FetchType Lazy 전략 사용

```java
@Entity
@Table(name = "member")
public class Member {

    ...
    @Basic(fetch = FetchType.LAZY)
    @Embedded
    private Favorites favorites;
    
    ...
}
```
