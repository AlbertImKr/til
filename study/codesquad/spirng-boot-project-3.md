# Spirng Boot Project 3주차 회고

이번 주 키워드는 대부분이 내용이 많고 어려워서 구현을 위해 참고한 링크만 정리했습니다. 시간이 될 때 하나씩 정리하려고 합니다.

## 1. 왜 JPA를 사용하는 가?

{% content-ref url="../db/why-jpa.md" %}
[why-jpa.md](../db/why-jpa.md)
{% endcontent-ref %}

{% content-ref url="../db/between-jpa-and-jdbc.md" %}
[between-jpa-and-jdbc.md](../db/between-jpa-and-jdbc.md)
{% endcontent-ref %}

## 2. 디미터 법칙

* 디미터 법칙이란?

{% content-ref url="../cs/demeter_law.md" %}
[demeter\_law.md](../cs/demeter\_law.md)
{% endcontent-ref %}

* 문제 코드

```java
(!joinForm.getPassword().equals(joinForm.getReconfirmPassword()))
```

* 개선후

```java
(!joinForm.isSamePassword())
```

## 3. Bean Validation

{% embed url="https://reflectoring.io/bean-validation-with-spring-boot/" %}

{% embed url="https://meetup.nhncloud.com/posts/223" %}

{% embed url="https://reflectoring.io/bean-validation-anti-patterns/" %}

{% embed url="https://stackoverflow.com/questions/52299412/javax-annotation-constrainst-reportassingleviolation" %}

{% embed url="https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#section-fail-fast" %}

## 4. AOP

{% embed url="https://jojoldu.tistory.com/71" %}

{% embed url="https://www.baeldung.com/spring-aop-advice-tutorial" %}

{% embed url="https://www.baeldung.com/spring-aop-vs-aspectj" %}

## 5. `@Autowired` vs Constructor Dependency Injection

{% content-ref url="../spring/autowired-vs-constructor-dependency-injection.md" %}
[autowired-vs-constructor-dependency-injection.md](../spring/autowired-vs-constructor-dependency-injection.md)
{% endcontent-ref %}

## 6. 오류 코드

{% content-ref url="../cs/erro_code.md" %}
[erro\_code.md](../cs/erro\_code.md)
{% endcontent-ref %}

## 7. JPA 테이블명 네이밍 전략

{% embed url="https://www.baeldung.com/spring-data-jpa-custom-naming" %}

## 8.Entity 에서 `@NotNull` 필드를 삭제

#### DataIntegrityViolationException  발생

```java
2023-04-18 06:08:39.320  WARN 67167 --- [nio-8080-exec-2] o.h.engine.jdbc.spi.SqlExceptionHelper   : SQL Error: 23503, SQLState: 23503
2023-04-18 06:08:39.320 ERROR 67167 --- [nio-8080-exec-2] o.h.engine.jdbc.spi.SqlExceptionHelper   : Referential integrity constraint violation: "FKE5HJEWHND6TRRDGT8I6UAPKHY: PUBLIC.POST FOREIGN KEY(ACCOUNT_ID) REFERENCES PUBLIC.ACCOUNT(ACCOUNT_ID) (CAST(2 AS BIGINT))"; SQL statement:
delete from account where account_id=? [23503-214]
2023-04-18 06:08:39.321  INFO 67167 --- [nio-8080-exec-2] o.h.e.j.b.internal.AbstractBatchImpl     : HHH000010: On release of batch it still contained JDBC statements
2023-04-18 06:08:39.324  WARN 67167 --- [nio-8080-exec-2] k.c.cafe.global.GlobalExceptionHandler   : request:url /user/delete
2023-04-18 06:08:39.324  WARN 67167 --- [nio-8080-exec-2] .m.m.a.ExceptionHandlerExceptionResolver : Resolved [org.springframework.dao.DataIntegrityViolationException: could not execute statement; SQL [n/a]; constraint ["FKE5HJEWHND6TRRDGT8I6UAPKHY: PUBLIC.POST FOREIGN KEY(ACCOUNT_ID) REFERENCES PUBLIC.ACCOUNT(ACCOUNT_ID) (CAST(2 AS BIGINT))"; SQL statement:<EOL>delete from account where account_id=? [23503-214]]; nested exception is org.hibernate.exception.ConstraintViolationException: could not execute statement]
```

## 9. 트랜잭션의 ACID 속성

{% content-ref url="../db/acid-properties-of-transactions.md" %}
[acid-properties-of-transactions.md](../db/acid-properties-of-transactions.md)
{% endcontent-ref %}

## 10. spring.jpa.open-in-view?

{% embed url="https://www.baeldung.com/spring-open-session-in-view" %}

## 11. assertSoftly

{% embed url="https://github.com/assertj/assertj-examples/blob/main/assertions-examples/src/test/java/org/assertj/examples/SoftAssertionsExamples.java" %}

