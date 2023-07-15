---
description: 페이징 처리하기
---

# Paging

## 목록을 보여줄 때 전체 데이터 중 일부만 보여주는 페이징 처리

```java
List<MemberData> findByNameLike(String name, Pageable pageable);
```

## `PageRequest.of()`

```java
PageRequest pageReq = PageRequest.of(1,10);
```

첫 번쨰 인자는

* 페이지 번호

두 번째 인자는

* 한 페이지의 개수

### PageRequset와 Sort를 사용하여 정렬 순서를 저정

```java
List<MemberData> user = memberDataDao.findByNameLike("사용자%", pageReq);
```

## Page가 제공하는 메서드의 일부

```java
Pageable pageReq = PageRequest.of(2,3);
Page<MemberData> page = memberDataDao.findByBlocked(false,pageReq);
List<MemberData> content = page.getContent(); // 조회 결과 목록
long totalElements = page.getTotalElements(); // 조건에 해당하는 전체 개수
int totalPages = page.getTotalPages(); // 전체 페이지 번호
int number = page.getNumber(); // 현재 페이지 번호
int numberOfElements = page.getNumberOfElements(); // 조회 결과 개수
int size = page.getSize(); // 페이지 크기
```

{% hint style="info" %}
프로퍼티를 비교하는 findBy프로퍼티 형식의 메서드의 Pageable 타입을 사용하더라도 리턴 타입이 List면 COUNT 쿼리를 실행하지 않는다.&#x20;

<mark style="color:purple;">List\<MemberData> findByNameLike(String name, Pageable pageable);</mark>

반면에 스펙을 사용하는 findAll 메서드에 Pageable 타입을 사용하면 리턴 타입이 Page가 아니여도 COUNT 쿼리를 실행한다.

<mark style="color:purple;">List\<MemberData> findAll(Specification\<MemberData> spec, Pageable pageable);</mark>
{% endhint %}

## findFirstN

처음 부터 N개의 데이터가 필요하다면 findFirstN 형식의 메서드를 사용할 수도 있다

```java
List<MemberData> findFirst3ByNameLikeOrderByName(String name)
```

## Reference

[도메인 주도 개발 시작하기: DDD 핵심 개념 정리부터 구현까지](https://product.kyobobook.co.kr/detail/S000001810495)
