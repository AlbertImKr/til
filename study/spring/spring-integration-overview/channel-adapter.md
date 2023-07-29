# Channel Adapter

채널 어댑터는 단일 발신자 또는 수신자를 메시지 채널에 연결할 수 있게 해주는 메시지 엔드포인트이다.

## **Configuring An Inbound Channel Adapter**

인바운드 채널 어댑터 요소(자바 구성의 **SourcePollingChannelAdapter**)는 Spring 관리 객체의 모든 메서드를 호출하고 메서드의 출력을 Message로 **변환**한 후 null이 아닌 반환 값을 **MessageChannel**로 보낼 수 있습니다.

어댑터의 구독이 활성화되면 **poller**는 소스에서 메시지 수신을 시도합니다.

**poller**는 제공된 구성에 따라 **TaskScheduler**로 예약됩니다

**개별 채널 어댑터**에 대한 **폴링** 간격 또는 cron 표현식을 구성하려면 **fixed-rate** 또는 **cron**과 같은 일정 속성 중 하나와 함께 **poller** 요소를 제공할 수 있습니다.

#### 다음 **예**에서는 두 개의 인바운드 채널 어댑터 인스턴스를 정의합니다.

```java
@Bean
public IntegrationFlow source1() {
    return IntegrationFlow.from(() -> new GenericMessage<>(...),
                             e -> e.poller(p -> p.fixedRate(5000)))
                ...
                .get();
}

@Bean
public IntegrationFlow source2() {
    return IntegrationFlow.from(() -> new GenericMessage<>(...),
                             e -> e.poller(p -> p.cron("30 * 9-17 * * MON-FRI")))
                ...
                .get();
}
```

## **Configuring An Outbound Channel Adapter**

**`outbound-channel-adapter`** 요소(Java 구성용 **`@ServiceActivator`**)는 해당 채널로 전송된 메시지의 페이로드와 함께 호출되어야 하는 POJO 소비자 메서드에 **`MessageChannel`**을 연결할 수도 있습니다.

다음 예는 아웃바운드 채널 어댑터를 정의하는 방법을 보여줍니다.

```java
@Bean
public IntegrationFlow outboundChannelAdapterFlow(MyPojo myPojo) {
    return f -> f
             .handle(myPojo, "handle");
}
```

조정 중인 채널이 PollableChannel인 경우 다음 예제와 같이 **poller** 하위 요소(`@ServiceActivator`의 `@Poller` 하위 주석)를 제공해야 합니다.

```java
public class MyPojo {

    @ServiceActivator(channel = "channel1", poller = @Poller(fixedRate = "3000"))
    void handle(Object payload) {
        ...
    }

}
```

## **Channel Adapter Expressions and Scripts**

다른 많은 Spring 통합 구성 요소와 마찬가지로 `<inbound-channel-adapter>`및 `<outbound-channel-adapte>`도 SpEL 표현식 평가를 지원합니다.
