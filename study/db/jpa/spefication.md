---
description: 스펙
---

# Spefication

## 스펙

* 애그리거트가 특정 조건을 충족하는지를 검사할 때 사용하는 인터페이스

```java
Public interface Speficiation<T> {
    public boolean isSatisfiedBy(T agg)
}
```

* agg in 리포지터리
  * 애그리거트 루트
* agg in DAO
  * 검색 결과로 리터할 데이터 객체

## Spring Data JPA

* Specification 인터페이스

```java
public interface Specification<T> extends Serializable {
    // not,where,and,or 메서드 생략
    @Nullable
    Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder cb);
}
```

## JPA 정적 메타 모델

* JPA(Java Persistence API)에서 정적 메타 모델은 엔티티 클래스에 대한 메타 정보를 나타내는 Java 클래스입니다. 이 정적 메타 모델을 사용하면 컴파일 타임에 엔티티 속성을 안전하게 참조할 수 있고, 오타나 잘못된 속성 참조로 인한 런타임 오류를 방지할 수 있습니다.
* 정적 메타 모델은 `@StaticMetamodel`  애너테이션을 이용해서 관련 모델을 지정한다.
* 메타 모델 클래스는 모델 클래스의 이름 뒤에 '\_'을 붙인 이름을 갖는다.
* 정적 메타 모델 클래스는 대상 모델의 각 프로퍼티와 동일한 이름을 갖는 정적 필드를 정의한다. 이 정적 필드는 프로퍼티에 대한 메타 모델로서 프로퍼티 타입에 따라 SingularAttribute, ListAttribute 등의 타입을 사용해서 메타 모델을 정의한다.

### 정적 메타 모델의 용도:

1. 안전한 속성 접근: 정적 메타 모델을 사용하면 컴파일 타임에 속성에 대한 오타나 잘못된 참조를 확인할 수 있습니다. IDE의 자동 완성 기능을 활용하여 올바른 속성을 선택하고, 컴파일러의 오류 검사를 통해 잘못된 속성 접근을 방지할 수 있습니다.
2. 쿼리 작성: 정적 메타 모델은 JPA 쿼리 작성에 사용됩니다. 예를 들어, JPA Criteria API나 JPQL에서 엔티티 속성을 사용하는 경우 정적 메타 모델을 활용하여 안전하게 속성에 접근할 수 있습니다. 이는 동적인 문자열로 속성을 참조하는 것보다 컴파일 타임에 오류를 잡을 수 있으며, 유지보수와 디버깅에 도움이 됩니다.
3. 프레임워크와 툴의 지원: JPA 구현체나 다른 관련 도구들은 정적 메타 모델을 활용하여 다양한 기능과 최적화를 제공합니다. 예를 들어, 쿼리 성능 향상을 위한 인덱스 생성이나 프로젝션 쿼리 등에서 정적 메타 모델을 활용할 수 있습니다.

## 스펙 조합

### 스펙 인터페이스는 `and`와 `or` 메서드 제공&#x20;

```java
public interface Sepcification<T> extends Serializable {
    ...생략
    
    default Specification<T> and(@Nullable Specification<T> other) { ... }
    default Specification<T> or(@Nullable Specification<T> other) { ... }
}
```

### `and()`

* 두 스펙을 모두 충족하는 조건을 표현하는 스펙을 생성

### `or()`

* 두 스펙 중 하나 이상 충족하는 조건을 표현하는 스펙을 생성

### `not()`

* 조건을 반대로 적용할 때 사용한다

### `where()`

* 스펙 인터페이스의 정적 메서드로 null을 전달하면 아무 조건도 생성하지 않는 스펙 객체를 리턴하고 null이 아니면 인자로 받은 스펙 객체를 그대로 리턴한다.

```java
// Where 사용하지 않으면
Specification<OrderSummary> spec = 
    nullableSpec == null ? otherSpec : nullableSpec.and(otherSpec);

// Where 문으로 NullPointerException 방지
Specification<OrderSummary> spec = 
    Specification.where(createNullableSpec()).and(createOtherSpec());
```

## 리포지터리/DAO 에서 스펙 사용하기

### 예시

```java
Public interface OrderSummaryDAO extends Repository<OrderSummary,String>{
    List<OrderSummary> findAll(Specification<OrderSummary> spec)
}

```

## 스펙 조합을 위한 스펙 빌더 클래스

if와 각 스텍을 조합하는 코드가 섞여 있어 실수하기 좋고 복잡한 구조를 갖는다. 스펙 빌더를 만들어 사용할 수 있다.

```
Specification<MemberData> spec = SpecBuilder.builder(MemberData.class)
    .isTrue(...,() -> ...)
    .isHasText(...,() -> ...)
    .toSpec();
```

## Reference

[도메인 주도 개발 시작하기: DDD 핵심 개념 정리부터 구현까지](https://product.kyobobook.co.kr/detail/S000001810495)
