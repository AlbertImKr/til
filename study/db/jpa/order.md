---
description: 정렬
---

# Order

## 스프링 데이터 JPA는 두가지 방법

### 메서드 이름에 OrderBy를  사용해서 정렬 기준 지정

* 사용하는 방법은 간단하지만 정렬 기준 프로퍼티가 두개 이상이면 메서드 이름이 길어지는 단점이 있다

#### 예시

```java
List<OrderSummary> findByOrderIdOrderByNumberDesc(String orderId);
```

### Sort를 인자로 전달

* 스프링 데이터 JPA는 정렬 순서는 지정할 때 사용할 수 있는 Sort 타입을 제공한다.

#### 예시

```java
List<OrderSummary> findByOrderId(String orderId,Sort sort):
```

## Reference

[도메인 주도 개발 시작하기: DDD 핵심 개념 정리부터 구현까지](https://product.kyobobook.co.kr/detail/S000001810495)
