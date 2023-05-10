# Identifiers in Hibernate/JPA

## Generated Identifiers

* 기본 키 값을 자동으로 생성하려면 `@GeneratedValue` annotation을 추가한다,

## 4가지 생성 유형을 사용할 수 있습니다.

* AUTO:&#x20;
  * 기본 generation type를 사용하는 경우 persistence provider는 primary key attribute의 type을 기준으로 값을 결정한다. 이 type는 numerical 혹은 UUID일수 있습니다.
  * numerical 값의 경우 generation은 sequence 혹은 table generator를 기반으로 한다.
    * primary key 값은은 데이터베이스 level에서 unique한다.
  * UUID 값은 UUIDgenerator를 사용한다.
* IDENTITY:
  * database의 identity column에서 생성하는 값을 의존한다.
* SEQUENCE:
  * sequence-based id를 사용하기 위해 Hibernate는 SequenceStyleGenerator 를 제공한다.
  * 데이터베이스가 sequences를 지원하는 경우 sequences를 사용하고 지원하지 않으면 Table Generation으로 switch한다.
  * 생성된 값은 sequence마다 unique합니다. sequence 이름을 지정하지 않으면 Hibernate는 다른 유형에 대해 동일한 hibernate\_sequence를 재사용합니다.
* TABLE:
  * 데이터베이스의 특정 테이블을 사용하여 값을 생성합니다.

{% embed url="https://www.baeldung.com/hibernate-identifiers" %}
