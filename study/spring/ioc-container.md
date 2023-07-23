# IoC Container

## POJO(Plain Old Java Object)

* 오래된 방식의 단순 자바 객체, 특정 프레임워크에 대한 참조가 없는 일반 Java 객체입니다.간단하고 가벼운 자바 객체를 가리키는데 사용되는 용어입니다.
* POJO는 속성 및 메서드에 대한 명명 규칙을 사용하지 않습니다.

### JavaBeans

* JavaBean은 본질적으로 POJO이므로 JavaBean은 대부분 POJO와 유사하며 이를 구현하는 방법에 대한 몇 가지 엄격한 규칙 집합이 있습니다.
* 규칙은 직렬화 가능하고 null 생성자를 가지며 getX() 및 setX() 규칙을 따르는 메서드를 사용하여 변수에 대한 액세스를 허용하도록 지정합니다.

### DTO Pattern

* Data Transfer Object
* DTO는 프로세스 또는 네트워크 간에 데이터를 전달하기 위해 값을 캡슐화합니다.
  * 호출하는 메서드 수를 줄이고 여러 매개변수 또는 값을 포함함으로써 원격 작업에서 네트워크 오버헤드를 줄입니다.

### VO

* Value Object
* equals()와 hashCode() 메서드를 override
* 일반적으로 숫자, 날짜 ,문자열 등과 같은 작은 개체를 캡슐화합니다.
* 불변을 많이 사용한다.
* 스프링 프레임워크의 가장 중요한 의의가 이 POJO로 자바 애플리케이션을 개발하는 것이므로 스프링의 주요 기능은 대부분 IoC 컨테이너 안에서 POJO를 구성 및 관리하는 일과 연관돼 있습니다.

## 리플랙션 프로그래밍

* 성찰적 프로그래밍 또는 성찰은 자체 구조와 동작을 검사, 성찰 및 수정하는 프로세스의 능력입니다.
* Java와 같은 객체 지향 프로그램잉 언어에서 리플레션을 사용하면 컴파일 시간에 인터페이스, 필드, 메소드 이름을 몰라도 런타임에 클래스,인터페이스,필드 및 메서드를 검사 할수 있습니다. 또한 새 개체의 인스턴스화 및 메서드 호출을 허용합니다.
* Java 리플렉션은 이름별로 클래스 및 데이터 구조에 대한 정보의 동적 검색을 지원하고 실행 중인 Java 프로그램 내에서 조작할 수 있기 때문에 유용합니다.
* 리플렉션은 Java 프로그래밍 언어의 기능입니다. 이를 통해 실행 중인 Java 프로그램이 자체적으로 검사하거나 "검사"하고 프로그램의 내부 속성을 조작할 수 있습니다.

### Runtime vs. Compile Time

*   일반적으로 컴퓨터 프로그래밍에는 여러 단계의 소프트웨어 프로그램 개발이 있습니다.

    <figure><img src="../../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>
* Compile Time
  * Java와 같은 고급 언어로 작성된 명령이나 소스 코드를 컴퓨터가 이해할 수 있는 기계 코드로 변환되여야 한다. 커파일 시간 동안 소스 코드는 .java에서 .class로의 바이트 코드로 변환됩니다. 커파일 시간 동안 컴파일러는 코드의 구문,의미 체계 및 유형을 확인합니다.
  * 구문과 의미를 체크(sytax and semantic)
  * 프로그맴 실행전
* Runtime
  * 프로그램의 수명 주기는 프로그램이 실행 중인 런탕입니다.
  * 프로그램 실행후
* 리플랙션을 지원하는 언어는 런탕임에 사용할 수 있는 여러 기능을 제공힌다
  * 러탕임에 소스 코드 구성(예: 코드 불록, 클래스 ,메소드 ,프로토콜 등)을 1등급 개체로 검색하고 수정합니다.
  * 클래스 또는 함수의 기호 이름과 일치하는 문자열을 해당 클래스 또는 함수에 대한 참조 또는 호출로 변환합니다.
  * 런타임 시 소스 코드 문인 것처럼 문자열을 평가합니다.
  * 프로그래밍 구조에 대한 새로운 의미나 목적을 부여하기 위해 언어의 바이트코드에 대한 새 인터프리터를 만듭니다.

## IoC 컨테이너를 초기화하여 애너테이션 스캐닝하기

* 애너테이션을 붙인 자바 클래스를 스캐닝하려면 우선 IoC 컨테이너를 인스턴스화(instantiation)해야 한다.
  * 그래야 스프링이 @Configuration,@Bean을 발견하고 나중에 IoC 컨테이너에서 빈 인스터스를 가져올 수 있다.
* 스프링은 기본 구현체인 빈 팩토리(bean factory)와 이와 호환되는 고급 구현체인 애플리케이션 컨텍스트(application context), 두가지 IoC 컨테이너를 제공합니다.
  * 애플리케이션 컨텍스트는 기본 기능에 충실하면서도 빈 팩토리보다 (스프링을 애플릿이나 모바일 기기에서 실행하는 등) 발전된 기능을 지니고 있으므로 리소스에 제약을 받는 상황이 아니라면 가급적 애플리케이션 컨텍스를 사용하는 게 좋습니다.
  * BeanFactory와 ApplicationContext는 각각 빈 팩토리와 애플리케이션 컨텍스트에 접근하는 인터페이스입니다.
    * ApplicationContext는 BeanFactory의 하위 인터페이스에서 호환성이 보장됩니다.
* IoC 컨테이너에서 POJO 인스턴스/빈 가져오기
  * 구성 클래스에 선언된 빈을 빈 팩토리 또는 애플리케이션 컨텍스트에서 가져오려면 유일한 빈 이름을 getBean() 메서드의 인수로 호출합니다.
* POJO 클래스 @Component를 붙여 DAO 빈 생성하기
  * 처음애는 구성 클래스에서 값을 하드코딩해 스프링 빈을 인스턴스화했습니다.
  * 실제로 POJO는 대부분 DB나 유저 입력을 활용해 인스턴스로 만듭니다.
  * Domain 클래스 및 DAO(Data Access Object) 데이터 액세스 객체) 를 이용해 POJO를 생성합니다.

### @Component

* 퍼시스턴스 (persisitence) (영속화)
* 서비스 (service)
* 프레젠테이션 (presentation) (표현)
* 세 레이어 layer (계층)

### @Autowired

* @Resource
* @Inject

### @Scope

| 스코프           | 설명                                                    |
| ------------- | ----------------------------------------------------- |
| singleton     | IoC 컨테이너당 빈 인스턴스 하나를 생성합니다.                           |
| prototype     | 요청할 때마다 빈 인스턴스를 새로 만듭니다.                              |
| request       | HTTP 요청당 하나의 빈 인스턴스를 생성합니다. 웹 애플리케이션 컨텍스트에만 해당됩니다.    |
| session       | HTTP 세션당 빈 인스턴스 하나를 생성합니다. 웹 애플리케이션 컨텍스트에만 해당됩니다.     |
| globalSession | 전역 HTTP 세션당 빈 인스턴스 하나를 생성합니다. 포털 애플리케이션 컨텍스트에만 해당됩니다. |

### IoC

* service locator pattern
  * 빈등록 및 생성
* dependency injection(의존성 주입)
  * Without dependency injection
  * Constructor injection
  * Interface injection
  * Assembly
* contextualized lookup(상황에 맞는 조회)
* template method design pattern(템플릿 메소드 패턴)
* strategy design pattern(저략 디자인 패턴)

### Spring 컨테이너 Configuration Metadata

* XML-based metadata
* Annotation-based configuration
  * define beans using annotation-based configuration metadata
* Java-based configuration
  * define beans external to your application classes by using Java rather than XML files.

### Lifecycle Callbacks

* @PostConstruct
  * 구성후
* @PreDestroy
  * 산전 파괴

## Reference

1. [https://www.baeldung.com/java-pojo-javabeans-dto-vo](https://www.baeldung.com/java-pojo-javabeans-dto-vo)
2. [https://www.baeldung.com/java-pojo-class](https://www.baeldung.com/java-pojo-class)
3. [https://en.wikipedia.org/wiki/Reflective\_programming](https://en.wikipedia.org/wiki/Reflective\_programming)
4. [https://www.oracle.com/java/technologies/service-locator.html](https://www.oracle.com/java/technologies/service-locator.html)
5. [https://en.wikipedia.org/wiki/Dependency\_injection](https://en.wikipedia.org/wiki/Dependency\_injection)
6. [https://springframework.guru/service-locator-pattern-in-spring/](https://springframework.guru/service-locator-pattern-in-spring/)
