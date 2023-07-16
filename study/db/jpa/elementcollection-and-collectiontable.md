---
description: 엔티티와 분리된 일반적인 값 객체들을 매핑
---

# ElementCollection\&CollectionTable

\
`@CollectionTable`은 `@ElementCollection`과 함께 사용되는 어노테이션으로, 매핑된 값 객체의 데이터를 저장하기 위한 별도의 매핑 테이블을 정의하는 데 사용됩니다.`@CollectionTable`을 사용하여 매핑 테이블을 정의하면 `@ElementCollection`에 의해 매핑된 값 객체의 데이터가 해당 테이블에 저장됩니다

## 사용 예시

```java
@ElementCollection
private List<String> emails;
```

## 컬렉션 종류

`List`, `Set`, `Collection`, `Map`

## 값 타입

기본적인 자바 데이터 타입이나 사용자 정의 값 객체를 값 타입으로 사용할 수 있습니다. 값 객체는 `@Embeddable` 어노테이션이 적용된 클래스여야 합니다.

## 매핑 테이블

매핑된 값 객체의 데이터를 저장하기 위한 별도의 매핑 테이블을 생성합니다. 기본적으로 매핑 테이블은 매핑 소유자(엔티티)의 테이블과 연결됩니다.

## Join 컬럼

`@CollectionTable` 에서 `joinColumns`를 사용하여 외래 키 관계를 정의할 수 있습니다.

## Fetch 전략

`@ElementCollection`기본적으로는 `FetchType.EAGER`로 설정되어 있지만, `FetchType.LAZY`로 설정하여 지연 로딩을 사용할 수도 있습니다.

## Table Name

`@CollectionTable`의  `name` 속성을 사용하여 맹핑 테이블의 이름을 지정할 수 있다.

기본적으로는 소유자 엔티티의 이름과 컬렉션 속성의 이름을 조합하여 자동으로 테이블 이름이 생성된다.

## 유니크 제약 조건

`@CollectionTable`의 `uniqueConstrains` 속성을 사용하여 매핑 테이블에 유니크 제약 조건을 정의할 수 있다.&#x20;

`@UniqueConstraint` 어노테이션을 사용하여 유니크 제약 조건의 이름, 컬럼 등을 설정할 수 있다.

## 기타 속성

