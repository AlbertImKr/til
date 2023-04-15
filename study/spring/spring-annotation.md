# Spring Annotation

## Bean Annotations

### `@Component`

* 모든 Spring 관리 구성 요소를 표시한다.

### `@Service`&#x20;

* 서비스 계층의 클래스를 표시하는 에노테이션입니다.
* 비즈니스 로직을 보유하고 있음을 나타내기 위해서 입니다.

### `@Repository`

* 데이터베이스 저장소 역할을 하는 지속성 계층의 클래스에 추가합니다.
* 역할은 지속성 관련 예외를 포착하여 Spring의 통합된 **unchecked exceptions**로 다시 발생시키는 것입니다.

### `@Component Scanning`

* 스캔할 annotation configuration 클래스의 패키지를 구성한다
* basePackages 또는 값 인수(value는 basePackages의 별칭) 중 하나를 사용하여 기본 패키지 이름을 직접 지정할 수 있습니다.&#x20;

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
class VehicleFactoryConfig {}

@Configuration
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

* 인수를 지정하지 않으면 @ComponentScan annotation 클래스가 있는 동일한 패키지에서 스캔이 발생합니다.

### `@Controller`

* Controller 계층의 클래스를 표시하는 에노테이션입니다.

### `@Configuraion`

* `@Bean`이 달려있는 dean definition 메서드를 포함할 수 있습니다.

## Web Annotations

### `@RequestMapping`

* `@Controller` 클래스 내부에 **request handler** 메서드를 표시합니다.
  * _path,_ or its aliases, _name,_ and _value:_ which URL the method is mapped to
  * _method:_ compatible HTTP methods
  * _params:_ filters requests based on presence, absence, or value of HTTP parameters
  * _headers:_ filters requests based on presence, absence, or value of HTTP headers
  * _consumes:_ which media types the method can consume in the HTTP request body
  * _produces:_ which media types the method can produce in the HTTP response body

### `@RequestBody`

* maps the body of the HTTP request to an object:
  * 역직렬화는 자동이며 요청의 콘텐츠 유형에 따라 다릅니다.

### `@PathVariable`

* 이 annotation은 메서드 인수가 URI 템플릿 변수에 바인딩되어 있음을 나타냅니다.

### `@RequsetParam`

* accessing HTTP request parameters

### `@ResponseBody`

* Spring은 메소드의 결과를 응답으로 취급합니다.

### `@ExceptionHandler`

* 지정된 예외를 처리하는 지정한 오류 처리기 메서드를 선언할 수 있습니다.

### `@ResponseStatus`

* 원하는 응답 HTTP 상태를 지정할 수 있습니다.

### `@RestController`

* combines @Controller and @ResponseBody.

### `@ModelAttribute`

* 이미 모델에 있는 요소에 액세스 한다.

## Core Annotations

### `@Autowired`

* Spring이 주입할 의존성을 표시합니다.

### `@Bean`

* `@Bean` marks a factory method which instantiates a Spring bean

### `@Qualifier`

* 모호한 상황에서 사용하려는 bean id 또는 bean 이름을 제공하기 위해 @Autowired와 함께 @Qualifier를 사용합니다.

### `@Required`

* XML을 통해 채우려는 의존성을 표시하기 위한 setter 메서드에 `@Required`을 사용한다.

### `@Value`

* 속성 값을 Bean에 주입 할 수 있다.

### `@DependsOn`

* annotation에서 설정한 bean을 먼저 초기화 후 annotation 달린 bean을 초기화한다.

### `@Lazy`

* 응용 프로그램 시작 시가 아니라 요청 시 bean을 생성할 수 있습니다.

### `@Lookup`

* @Lookup 어노테이션이 붙은 메소드는 호출할 때 메소드의 리턴 유형 인스턴스를 리턴하도록 Spring에 지시합니다.

```java
@Component
@Scope("prototype")
public class SchoolNotification {
    // ... prototype-scoped state
}
```

```java
@Component
public class StudentServices {

    // ... member variables, etc.

    @Lookup
    public SchoolNotification getNotification() {
        return null;
    }

    // ... getters and setters
}
```

### `@Primary`

* 동일한 유형의 여러 bean중 특정 bean을 사용시 사용합니다.

### `@Scope`

* @Scope를 사용하여 @Component 클래스 또는 @Bean 정의의 범위를 정의합니다.

### `@Profile`

* 특정 프로필이 활성화된 경우에만 Spring이 @Component 클래스 또는 @Bean 메서드를 사용하도록 하려면 @Profile로 표시할 수 있습니다.

### `@Import`

* 이 annotation을 사용하여 구성 component scanning 없이 특정 `@Configuration`클래스를 사용할 수 있습니다.

### `@ImportResource`

* XML configurations을 가져올 수 있습니다.

### `@PropertySource`

* 애플리케이션 설정에 대한 속성 파일을 정의할 수 있습니다.

### `@PropertySources`

* @PropertySource configurations을 지정할 수 있습니다.





