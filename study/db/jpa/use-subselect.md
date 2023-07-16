---
description: 하이버네이트 @Subselect 사용
---

# Use Subselect

`@Immutable`,`@Subselcet`,`@Synchronize`는 하이버네이트 전용 애너테이션이며 이 테그를 사용하면 테이블이 아닌 쿼리를 결과를 `@Entity`로 매핑할 수 있다.

## `@Subselect`

`@Subselect`는 죄회(select) 쿼리를 값으로 갖는다. 하이버네이트는 이 select 쿼리의 결과를 매핑할 테이블처럼 사용한다. DBMS가 여러 테이블을 조인해서 조회한 결과를 한 테이블처럼 보여주기 위한 용도로 뷰를 사요하는 것 처럼 `@Subselect`를 사용하면 쿼리 실행 결과를 매핑 할 테이블처럼 사용한다



## `@immutable`&#x20;

`@Immutabl`을 사용하면 하이버네이트는 해당 앤티티의 매핑 필드/프로퍼티가 변경되도 DB에 반영하지 않고 무시한다.

* `@Subselct`를 이용한 `@Entity`의 매핑 필드를 수정하면 하이버네이트는 변경 내역을 반영하는 update 쿼리를 실행할 것이다. 그런데 매핑 한 테이블이 없으므로 에러가 발생한다.  이런 문제를 방지하기 위해 사용한다.&#x20;

## `@Synchronize`

`@Synchronize`는 해당 엔티티와 관련된 테이블 목록을 명시한다. 하이버네이트는 엔티티를 로딩하기 전에 지정한 테이블과 관련된 변경이 발생하면 플러시(flush)를 먼저 한다.

* 특별한 이유가 없으면 하이버네이트는 트랜잭션을 커밋하는 시점에 변경사항을 DB에 반형하므로 변경 내역을 아직 반영하지 않은 상태에서 조회하면 최신 값이 아닌 이전 값이 담기게 된다.

## 적용

### 문제

```java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
@Immutable
@Subselect("""
        select
             o.order_id,
             o.store_id as store_id,
             p.product_id as product_id,
             o.user_id as user_id,
             o.created_time,
             u.nickname as nickname,
             p.product_name,
             o.delivery_status,
             o.amount
        from
             order_products_id opi
                 left join purchase_order o
             on  o.order_id = opi.order_order_id
                 left join user u
             on o.user_id = u.user_id
                 left join product p
             on opi.product_id = p.product_id
        """
)
@Synchronize({"purchase_order", "order_products_id", "product","user"})
public class OrderData {
}
```

테스트 결과는 동일한 결과를 list로 return 받았다.

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

MySql 동일한 Query로 조회 결과 Product\_id는 다르다.

<figure><img src="../../../.gitbook/assets/스크린샷 2023-07-15 오후 9.12.15.png" alt=""><figcaption></figcaption></figure>

### 원인

StoreID를 PK로 정하여 하이버네이트는 동일한 data로 판단하여 DB를 조회하지 않고 영속성 컨텍스트에서 객체를 가져와서 발생한 문제다.

## Reference

[도메인 주도 개발 시작하기: DDD 핵심 개념 정리부터 구현까지](https://product.kyobobook.co.kr/detail/S000001810495)
