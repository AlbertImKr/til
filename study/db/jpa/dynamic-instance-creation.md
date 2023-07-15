---
description: 동적 인스턴스 생성
---

# Dynamic Instance Creation

## JPQL에서 동적 인스턴스를 사용한 코드

````java
public interface DAO extends Repository<Order,String> {
    
    @Query{```
           select new com.(...).Order(
              o.id,
              ...
           )
           from Order o join ...
           where ...
           ```
    }
    List<Order> findOrder(String orderId);
}
````

동적 인스턴스의 장점은 JPQL을 그대로 사용하므로 객체 기준으로 쿼리를 작성하면서도 종시에 지연/즉시 로딩과 같은 고민 없이 원하는 모습으로 데이터를 조회할 수 있다

## Reference

[도메인 주도 개발 시작하기: DDD 핵심 개념 정리부터 구현까지](https://product.kyobobook.co.kr/detail/S000001810495)
