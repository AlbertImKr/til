# Spring Boot Project 2주차 회고

## 1. [CQRS pattern](../cs/design-pattern/cqrs-pattern/)

* 아직 깊게 이해는 못했지만 개념을 찾아서 한번 알아봤습니다.

## 2.Test에서 `@Transactional`의 롤백

* Aoto Increment Column은 Roll Back 하지 않는다.
* auto increment 자동 증가 값을 생성하는 데 사용되는 잠금 모드와 관련이 있습니다.

> [https://stackoverflow.com/questions/449346/mysql-auto-increment-does-not-rollback](https://stackoverflow.com/questions/449346/mysql-auto-increment-does-not-rollback)

## 3.Test에서의 하드코딩 개선

존재 하지 않은 `UserId`에 대한 접근의 Test에서

```java
mockMvc.perform(put("/users/20/update")
```

`20`은 아직 존재하지 않는 `UserId`를 의미합니다. 개

개선후

```java
private static final int NON_EXISTING_USER_ID = 20;
mockMvc.perform(put("/users/"+NON_EXISTING_USER_ID+"/update")
```

가독성 향상

## 4.Logging Level 에 대한 재고민

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 아직 코드가 간단해서 다양한 레벨의 필요성을 느끼지 못하고 있다. 이해를 더 깊게 하려면 조금 더 복잡한 구조의 프로젝트로 만들고 이해를 하여야 할 것 같습니다.

> [https://stackoverflow.com/questions/2031163/when-to-use-the-different-log-levels](https://stackoverflow.com/questions/2031163/when-to-use-the-different-log-levels)

## 5.initBeanPropertyAccess() 와 initDirectFieldAccess()

* DataBinder 의 기본 설정은 `initBeanPropertyAccess()` 입니다. JavaBean property access로 객체를 초기화한다. 즉 setter 사용하여 초기화 한다.

```
// Some code
```

* `initDriectFiedlAccess()`로 설정하면 직접 필드를 access하여 초기화 한다. 즉 setter를 사용하지 않고 초기화 한다.
* 장점:
  1. 노출하고 싶은 부분만 외부에 노출
  2. 상태는 잘 캡슐화되어 있습니다.
* 단점:
  * 컨테이너는 값 주입을 위해 게터와 세터를 사용하지 않습니다. injection-process는 direct field injection으로 디버깅할 수 없습니다.
* Spring에서 설정방법(예시)

```java
@ControllerAdvice
public class WebControllerAdvice {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        binder.initDirectFieldAccess();
    }
}
```

> [https://www.logicbig.com/tutorials/spring-framework/spring-core/direct-field-access.html](https://www.logicbig.com/tutorials/spring-framework/spring-core/direct-field-access.html)

## 6.`@Valid`와 `@Validated`의 차이

* Spring에서는 JSR-303's `@Valid` annotation을 메서드 level 유효송 검사에 사용합니다. 또한 유효성 검사를 위해 member attribute를 표시하는데 사용합니다. 그러난 이 `@Valid`는 group 유효성 검사를 지원하지 않습니다.
* 그룹 level의 경우 JSR-303의 `@Valid`의 변형인 Spring의 `@Validated`를 사용해야 합니다.

> [https://www.baeldung.com/spring-valid-vs-validated](https://www.baeldung.com/spring-valid-vs-validated)
>
> [https://www.baeldung.com/javax-validation-groups](https://www.baeldung.com/javax-validation-groups)

## 7. jasypt를 통한 암호화

### 사용전

```
spring.datasource.url=jdbc:h2:~/test
spring.datasource.username=testname
spring.datasource.password=testpassword
```

### 사용후

```
spring.datasource.url=ENC(+WCZeeCfIhwJETdywT8GZvPJIVNnVqqoeKf3NComOUSEc4YqJz3PpdaEAYnWDzF3ij73yn3GnjU=)
spring.datasource.username=ENC(flrTBReqIDeRl1zFOAKafA==)
spring.datasource.password=ENC(7pUsCOYOlr1Z8xSrPu6Ci2zufHayPbHW)
```

### 사용방법

#### Spring-boot 를 지원

* dependencies 추가

```
implementation 'com.github.ulisesbocchio:jasypt-spring-boot-starter:3.0.5'
```

* application.properties 설정 추가

```
//이 IvGenerator 구현은 항상 길이가 0인 초기화 벡터(IV)를 반환합니다.
jasypt.encryptor.iv-generator-classname=org.jasypt.iv.NoIvGenerator 
//암호화 알고리즘
jasypt.encryptor.algorithm=PBEWithMD5AndTripleDES 
//임의의 암호화 password(업로드 시 삭제,
//--jasypt.encryptor.password=dip3tH7IJrL 추가하여 Java 실행) 
jasypt.encryptor.password=dip3tH7IJrL
```

* 테스트에서 암호화 할 내용을 출력

```java
	@Autowired
	StringEncryptor stringEncryptor;

	@Test
	void contextLoads() {
		String encrypt = stringEncryptor.encrypt("jdbc:h2:~/test");
		System.out.println("encrypt = " + encrypt);
	}
```

* 출력한 암호를 복사

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* properties에서 ENC추가하여 암호화 완성

```
spring.datasource.url=ENC(w8FSUhvvlH5YMHPkm6SI3O3fbE0Efb0+)
```

* application.properties 설정에서 jasypt.encryptor.password 삭제하고 --jasypt.encryptor.password=dip3tH7IJrL 추가하여 Java 실행하면 끝

#### Spring Boot 내에서 사용

* StringEncryptor를 `@Autowired`하여 사용하면 됩니다.

```java
public class AppStartupRunner implements CommandLineRunner {

    @Autowired
    UserRepository userRepository;
    @Autowired
    StringEncryptor encryptor;


    @Override
    public void run(String... args) {
        User build = new User.Builder()
                .password(encryptor.decrypt("W5p8Dbc3f7sOjqaRc7WVp8inlukLB3LJ"))
                .email(encryptor.decrypt("R9gvxFpZK6xYikAdPrdy1mDtd3O+ag2L"))
                .nickname("admin")
                .role(Role.MANAGER).build();
        userRepository.save(build);
    }
}
```

## 8. 쿼리에서 `*`를 사용하는 것은 지양한다

* 불필요한 IO
  * `SELECT *`를 사용하면 무시해야할 불필요한 데이터를 반환할 수 있습니다. 그러나 해당 데이터를 가져오는 데 비용이 발생한다. 이로 인해 페이지에서 해당 데이터를 모두 읽을 것이기 때문에 DB 측에서 일부 낭비적인 IO 주기가 발생합니다.
* 네트워크 트래픽 증가
  * `SELECT *`는 클라이언트에 필요한 것보다 더 많은 데이터를 반환하므로 클라이언트는 더 많은 네트워크 대역폭을 사용합니다. 이러한 네트워크 대역폭의 증가는 또한 데이터가 클라이언트 애플리케이션(SSMS 또는 Java 애플리케이션 서버일 수 있음)에 도달하는 데 더 오랜 시간이 걸린다는 것을 의미합니다.
* 더 많은 애플리케이션 메모리
  * 이러한 데이터 증가로 인해 응용 프로그램은 사용되지 않지만 Microsoft SQL 서버 또는 연결 중인 다른 데이터베이스에서 오는 불필요한 데이터를 보관하기 위해 더 많은 메모리가 필요할 수 있습니다.
* ResultSet의 열 순서에 대한 의존성
  * 응용 프로그램에서 `SELECT *`쿼리를 사용하고 열 순서에 의존성이 있는 경우(그러면 안 됨) 새 열을 추가하거나 열 순서를 변경하면 결과 집합의 순서가 변경됩니다.
* 테이블에 새 열을 추가하는 동안 보기 중단
  * 뷰에서 SELECT \*를 사용할 때 새 열이 추가되고 이전 열이 테이블에서 제거되면 미묘한 버그가 생성됩니다. 왜? 보기가 중단되지 않고 잘못된 결과를 반환하기 시작하기 때문입니다.
  * 이를 방지하려면 뷰와 함께 항상 `WITHSCHEMABINDING it`을 사용해야 합니다. 이렇게 하면 뷰에서 `SELECT *`를 사용할 수도 없게 됩니다.
* JOIN 쿼리 충돌
  * JOIN 쿼리에서 SELECT \*를 사용하면 여러 테이블에 동일한 이름(예: 상태, 활성, 이름 등)의 열이 있을 때 복잡해질 수 있습니다
* 한 테이블에서 다른 테이블로 데이터 복사
  * 한 테이블에서 다른 테이블로 데이터를 복사하는 일반적인 방법인 INSERT .. SELECT 문에 `SELECT *`를 사용할 때 두 테이블 간에 열 순서가 동일하지 않으면 잘못된 데이터를 잘못된 열에 복사할 가능성이 있습니다.
  * 일부 프로그래머는 쿼리 파서가 정적 값의 유효성을 검사하기 위해 추가 작업을 수행해야 하기 때문에 EXISTS 코드에서 SELECT \* 대신 SELECT 1을 사용하는 것이 더 빠르다고 생각합니다.
  * 오래 전에는 사실이었을지 모르지만 요즘 파서는 SELECT 목록이 EXISTS 절 내에서 완전히 관련이 없다는 것을 알 만큼 똑똑해졌습니다.

## 9.jdbcTemplate와 namedparameter

* Spring Boot JDBC에서 DataSource, JdbcTemplate 및 NamedParameterJdbcTemplate과 같은 데이터베이스 관련 bean은 시작 중에 구성되고 생성되어 이를 사용합니다. 예를 들어 원하는 bean을 @Autowired하면 됩니다.

> [https://mkyong.com/spring-boot/spring-boot-jdbc-examples/](https://mkyong.com/spring-boot/spring-boot-jdbc-examples/)

## 10.exists 쿼리 성능 개선

* `select exists`를 `limit 1` 로 대체해서 사용한다.
* 단, JpaRepository의 메소드 쿼리에선 내부적으로 `limit 1`를 사용하고 있어서 성능상 이슈가 없다.

```sql
//지양
SELECT count(*) FROM users where id = :id
//추천
SELECT userName,.... FROM users where id = :id limit 1
```

> [https://jojoldu.tistory.com/516](https://jojoldu.tistory.com/516)

## 11.DB에 식별자(테이블 이름, 열 이름 등)로 사용할 수 없는 키워드 목록이 있습니다.

* 따옴표로 묶으면 사용가능하고 대소문자 구별합니다. 하지만 개인적으로 선호하지 않습니다. 회피는 결국 에러를 발생할 수 있다고 판단합니다.

> [http://www.h2database.com/html/advanced.html](http://www.h2database.com/html/advanced.html)

## 12.SQLErrorCodeSQLExceptionTranslator

`SQLExceptionTranslator` 는 `SQLExceptions`와 Spring의 자체 `org.springframework.dao.DataAccessException` 사이에서 변환할 수 있는 클래스에 의해 구현되는 인터페이스입니다. 이는 인식불가능한 data access와 관련되여 있다. 구현은 일반(예: JDBC용 SQLState 코드 사용) 또는 특정상황(예: Oracle 오류 코드 사용)에 정밀도를 높일 수 있습니다.

* SQLErrorCodeSQLExceptionTranslator는 다음 순서로 일치 규칙을 적용합니다.
  * 하위 클래스에 의해 구현된 모든 custom translation. 일반적으로 제공된 구체적인 `SQLErrorCodeSQLExceptionTranslator`가 사용되므로 이 규칙이 적용되지 않습니다. 실제로 하위 클래스 구현을 제공한 경우에만 적용됩니다.
  * `SQLErrorCodes` 클래스의 `customSqlExceptionTranslator`property으로 제공되는 `SQLExceptionTranslator` 인터페이스의 custom 구현.
  * `SQLErrorCodes` 클래스의 `customTranslations` 속성에 대해 제공된 `CustomSQLErrorCodesTranslation` 클래스의 인스턴스 목록에서 일치 항목을 검색합니다.
  * 오류 코드 일치가 적용됩니다
  * fallback translator를 사용한다. `SQLExceptionSubclassTranslator`는 default fallback translator입니다. 이 변환을 사용할 수 없는 경우 다음 fallback translator는 SQLStateSQLExceptionTranslator입니다.

> [https://docs.spring.io/spring-framework/docs/3.0.x/spring-framework-reference/html/jdbc.html#jdbc-SQLExceptionTranslator](https://docs.spring.io/spring-framework/docs/3.0.x/spring-framework-reference/html/jdbc.html#jdbc-SQLExceptionTranslator)
