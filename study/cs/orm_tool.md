---
description: 객체 관계 매핑 데이터베이스 도구
---

# ORM 도구

ORM(Object Relational Mapping)은  대부분의 경우 객체 지향 프로그램과  관계형 데이터베이스 사이에 "다리"를 만드는 데 사용되는 기술입니다.



다르게 말하면 ORM을 객체 지향 프로그래밍(OOP)을 관계형 데이터베이스에 연결하는 계층으로 볼 수 있습니다.



OOP 언어를 사용하여 데이터베이스와 상호 작용할 때 데이터베이스에서 데이터 생성, 읽기, 업데이트 및 삭제(CRUD)와 같은 다양한 작업을 수행해야 합니다.기본적으로 관계형 데이터베이스에서 이러한 작업을 수행하기 위해 SQL을 사용합니다.



이러한 목적으로 SQL을 사용하는 것이 반드시 나쁜 생각은 아니지만 ORM 및 ORM 도구는 관계형 데이터베이스와 다른 OOP 언어 간의 상호 작용을 단순화하는 데 도움이 됩니다.



## What is an ORM Tool?

ORM 도구는 OOP 개발자가 관계형 데이터베이스와 상호 작용할 수 있도록 설계된 소프트웨어입니다. 따라서 처음부터 자신만의 ORM 소프트웨어를 만드는 대신 이러한 도구를 사용할 수 있습니다.



다음은 데이터베이스에서 특정 사용자에 대한 정보를 검색하는 SQL 코드의 예입니다.

```sql
"SELECT id, name, email, country, phone_number FROM users WHERE id = 20"
```

위의 코드는 사용자에 대한 정보를 반환합니다. -`users` 테이블의 `name`,`email`,`country`과 `phone_number` .`WHERE` 절을 사용하여 정보가 `id`가 20인 사용자의 정보여야 함을 지정했습니다.



반면 ORM 도구는 더 간단한 방법으로 위와 동일한 쿼리를 수행할 수 있습니다. 다음과 같이:

```java
users.GetById(20)
```

이것은 위의 코드는 SQL 쿼리와 동일합니다. 모든 ORM 도구는 다르게 구축되므로 방법이 동일하지 않지만 일반적인 목적은 비슷합니다.



ORM 도구는 마지막 예제와 같은 메서드를 생성할 수 있습니다.



대부분의 OOP 언어에는 선택할 수 있는 다양한 ORM 도구가 있습니다. 다음은 Java, Python, PHP 및 .NET 개발에 가장 많이 사용되는 몇 가지입니다.



## Popular ORM Tools for Java <a href="#popular-orm-tools-for-java" id="popular-orm-tools-for-java"></a>

### **1.** [**Hibernate**](https://hibernate.org/orm/)

Hibernate를 사용하면 개발자는 상속, 다형성, 연결, 구성과 같은 OOP 개념에 따라 데이터 영구 클래스를 작성할 수 있습니다. Hibernate는 성능이 뛰어나고 확장 가능합니다.



### 2.[Apache OpenJpa](https://openjpa.apache.org/)

Apache OpenJPA는 Java persistence 도구이기도 합니다. 독립 실행형 POJO(Plain Old Java Object) persistence 계층으로 사용할 수 있습니다.



### 3.[EclipseLink](https://www.eclipse.org/eclipselink/)

EclipseLink는 관계형, XML 및 데이터베이스 웹 서비스를 위한 오픈 소스 Java persistence 솔루션입니다.



### 4.[jOOQ](https://www.jooq.org/)

jOOQ는 데이터베이스에 저장된 데이터에서 Java 코드를 생성합니다. 이 도구를 사용하여 형식이 안전한 SQL 쿼리를 작성할 수도 있습니다.



### 5.[Oracle TopLink](https://docs.oracle.com/cd/E17904\_01/web.1111/b32441/undtl.htm#JITDG91126)

Oracle TopLink를 사용하여 persistent 데이터를 저장하는 고성능 애플리케이션을 구축할 수 있습니다. 데이터는 관계형 데이터 또는 XML 요소로 변환될 수 있습니다.



## Advantages of Using ORM Tools <a href="#advantages-of-using-orm-tools" id="advantages-of-using-orm-tools"></a>

* 팀의 개발 시간을 단축합니다.
* 개발 비용 절감
* 데이터베이스와 상호 작용하는 데 필요한 논리를 처리합니다.
* 보안을 향상시킵니다. ORM 도구는 SQL 삽입 공격의 가능성을 제거하기 위해 만들어졌습니다.
* SQL보다 ORM 도구를 사용할 때 더 적은 코드를 작성할수 있습니다.



## Disadvantages of Using ORM Tools <a href="#disadvantages-of-using-orm-tools" id="disadvantages-of-using-orm-tools"></a>

* ORM 도구를 사용하는 방법을 배우는 데는 시간이 많이 걸릴 수 있습니다.
* 매우 복잡한 쿼리가 관련된 경우 더 나은 성능을 발휘하지 못할 가능성이 높습니다.
* ORM은 일반적으로 SQL을 사용하는 것보다 느립니다.







> [https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/](https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/)
