# Configuration and @EnableIntegration

## `@EnableIntegration`&#x20;

@EnableIntegration annotation은 애플리케이션 컨텍스트에 많은 인프라 구성 요소를 등록합니다.&#x20;

* **`errorChannel`** 및 해당 **`LoggingHandler`**, pollers용 **`taskScheduler`**, **`jsonPath SpEL-function`**, 등과 같은 일부 내장 빈을 등록합니다.
* 전역 및 기본 통합 환경을 위한 `BeanFactory`를 향상시키기 위해 여러 **`BeanFactoryPostProcessor`** 인스턴스를 추가합니다.
* 여러 **`BeanPostProcessor`** 인스턴스를 추가하여 통합 목적으로 특정 빈을 향상 또는 변환 및 래핑합니다.
* **메시징 annotation**을 구문 분석하기 위해 **annotation 프로세서**를 추가하고 응용 프로그램 컨텍스트에 대한 구성 요소를 등록합니다.

## `@IntegrationComponentScan`

Spring Integration과 관련된 구성 요소 및 주석으로 제한하여 스캔한다.(예: **`@MessagingGateway` )**

## `@EnablePublisher`

`@EnablePublisher` 어노테이션은 `PublisherAnnotationBeanPostProcessor` 빈을 등록하고 채널 속성 없이 제공되는 `@Publisher` 어노테이션에 대해 `default-publisher-channel`을 구성합니다

둘 이상의 `@EnablePublisher`주석이 발견되면 모두 기본 채널에 대해 동일한 값을 가져야 합니다.

## `@GlobalChannelInterceptor`

`@GlobalChannelInterceptor` 어노테이션은 글로벌 채널 차단을 위해 `ChannelInterceptor` 빈을 표시하기 위해 도입되었습니다.

`@GlobalChannelInterceptor` 어노테이션은 클래스 레벨(`@Component`스테레오타입 어노테이션 포함) 또는 `@Configuration` 클래스 내의 `@Bean` 메소드에 배치될 수 있습니다. 두 경우 모두 Bean은 ChannelInterceptor를 구현해야 합니다.

## `@IntegrationConverter`

`@IntegrationConverter` 주석은 `converter`, `GenericConverter` 또는 `ConverterFactory` 빈을 `integrationConversionService`의 후보 변환기로 표시합니다.

## Reference

{% embed url="https://docs.spring.io/spring-integration/docs/current/reference/html/overview.html#configuration-enable-integration" %}
