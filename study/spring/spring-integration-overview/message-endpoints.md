# Message Endpoints

## Message Transformer

메시지 변환기는 메시지의 **내용**이나 **구조**를 **변환**하고 수정된 **메시지**를 **반환**하는 역할을 합니다.

## **Message Filter**

메시지 필터는 메시지를 **출력** 채널로 **전달**해야 하는지 **여부**를 결정합니다.

메시지 필터는 여러 소비자가 동일한 메시지를 수신하고 필터 기준을 사용하여 처리할 메시지 집합의 범위를 좁힐 수 있는 **publish-subscribe channe**l과 함께 사용되는 경우가 많습니다.

## **Message Router**

메시지 라우터는 **다음**에 메시지를 수신해야 하는 **채널**(있는 경우)을 결정해야 합니다.

일반적으로 메시지 **내용** 또는 메시지 **헤더**에서 사용 가능한 **메타데이터**에 따라 결정됩니다.

메시지 라우터는 회신 메시지를 보낼 수 있는 서비스 액티베이터 또는 기타 엔드포인트에서 정적으로 구성된 출력 채널에 대한 **동적 대안**으로 자주 사용됩니다.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Message Router</p></figcaption></figure>

## **Splitter**

스플리터는 입력 채널에서 메시지를 수락하고 해당 메시지를 여러 메시지로 **분할**하고 각 메시지를 출력 채널로 보내는 또 다른 유형의 메시지 끝점입니다

## **Aggregator**

애그리게이터는 여러 메시지를 수신하여 단일 메시지로 결합하는 일종의 메시지 엔드포인트입니다.

## **Service Activator**

Service Activator는 서비스 인스턴스를 메시징 시스템에 연결하기 위한 일반 끝점입니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Service Activator</p></figcaption></figure>

{% hint style="info" %}
메시지 채널에서 채널은 폴링 가능하거나 구독 가능할 수 있습니다. 위 다이어그램에서 이것은 "**시계**" 기호와 실선 화살표(**poll**) 및 점선 화살표(**subscribe**)로 표시됩니다
{% endhint %}

## **Channel Adapter**

채널 어댑터는 메시지 채널을 다른 시스템이나 전송에 연결하는 끝점입니다.

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>인바운드 채널 어댑터 엔드포인트는 소스 시스템을 MessageChannel에 연결합니다.</p></figcaption></figure>



{% hint style="info" %}
메시지 소스는 **pollable**(예: POP3) 또는 **message-driven**(예: IMAP Idle) 할 수있니다. 앞의 다이어그램에서 이는 "시계" 기호와 실선 화살표(**poll**) 및 점선 화살표(**message-driven**)로 표시됩니다
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>아웃바운드 채널 어댑터 끝점은 MessageChannel을 대상 시스템에 연결합니다.</p></figcaption></figure>

{% hint style="info" %}
메시지 채널에서 채널은 폴링 가능하거나 구독 가능할 수 있습니다. 위 다이어그램에서 이것은 "**시계**" 기호와 실선 화살표(**poll**) 및 점선 화살표(**subscribe**)로 표시됩니다
{% endhint %}

## **Endpoint Bean Names**

소비 엔드포인트(inputChannel이 있는 모든 것)는 **consumer**와 **message** 핸들러라는 두 개의 빈으로 구성됩니다.

### **Consumer**는 메시지 핸들러에 대한 참조를 가지고 있으며 메시지가 도착하면 이를 호출합니다

#### XML 구성:

```xml
<int:service-activator id = "someService" ... />
```

앞의 예에서 bean 이름은 다음과 같습니다

* Consumer: `someService` (the `id`)
* Handler: `someService.handler`

#### EIP(Enterprise Integration Pattern) 어노테이션을 사용할 때 이름은 여러 요인에 따라 달라집니다. 어노테이션이 달린 POJO의 다음 예:

```java
@Component
public class SomeComponent {

    @ServiceActivator(inputChannel = ...)
    public String someMethod(...) {
        ...
    }

}

```

앞의 예에서 bean 이름은 다음과 같습니다

* Consumer: `someComponent.someMethod.serviceActivator`
* Handler: `someComponent.someMethod.serviceActivator.handler`

#### 버전 5.0.4부터 다음 예제와 같이 `@EndpointId`주석을 사용하여 이러한 이름을 수정할 수 있습니다.

```java
@Component
public class SomeComponent {

    @EndpointId("someService")
    @ServiceActivator(inputChannel = ...)
    public String someMethod(...) {
        ...
    }

}
```

앞의 예에서 bean 이름은 다음과 같습니다:

* Consumer: `someService`
* Handler: `someService.handler`

#### `@EndpointId`는 XML 구성을 사용하여 id 속성에 의해 생성된 이름을 생성합니다.

```java
@Configuration
public class SomeConfiguration {

    @Bean
    @ServiceActivator(inputChannel = ...)
    public MessageHandler someHandler() {
        ...
    }

}
```

앞의 예에서 bean 이름은 다음과 같습니다:

* Consumer: `someConfiguration.someHandler.serviceActivator`
* Handler: `someHandler` (the `@Bean` name)

#### 버전 5.0.4부터 다음 예제와 같이 @EndpointId 주석을 사용하여 이러한 이름을 수정할 수 있습니다.

```java
@Configuration
public class SomeConfiguration {

    @Bean("someService.handler")             (1)
    @EndpointId("someService")               (2)
    @ServiceActivator(inputChannel = ...)
    public MessageHandler someHandler() {
        ...
    }

}
```

앞의 예에서 bean 이름은 다음과 같습니다:

* Consumer: `someService` (the endpoint ID)
* Handler: `someService.handler` (the bean name)

### 마찬가지로 폴링 가능 메시지 소스는 SPCA(`SourcePollingChannelAdapter`)와 `MessageSource`라는 두 개의 빈을 생성합니다

#### XML 구성:

```xml
<int:inbound-channel-adapter id = "someAdapter" ... />
```

Given the preceding XML configuration, the bean names are as follows:

* SPCA: `someAdapter` (the `id`)
* Handler: `someAdapter.source`

#### Java configuration of a POJO to define an `@EndpointId`

```java
@EndpointId("someAdapter")
@InboundChannelAdapter(channel = "channel3", poller = @Poller(fixedDelay = "5000"))
public String pojoSource() {
    ...
}
```

Given the preceding Java configuration example, the bean names are as follows:

* SPCA: `someAdapter`
* Handler: `someAdapter.source`

#### Java configuration of a bean to define an `@EndpointID`:

```java
@Bean("someAdapter.source")
@EndpointId("someAdapter")
@InboundChannelAdapter(channel = "channel3", poller = @Poller(fixedDelay = "5000"))
public MessageSource<?> source() {
    return () -> {
        ...
    };
}
```

Given the preceding example, the bean names are as follows:

* SPCA: `someAdapter`
* Handler: `someAdapter.source` (as long as you use the convention of appending `.source` to the `@Bean` name)

## Reference

{% embed url="https://docs.spring.io/spring-integration/docs/current/reference/html/overview.html#overview-endpoints" %}
