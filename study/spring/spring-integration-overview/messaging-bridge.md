# Messaging Bridge

메시징 브리지는 두 개의 메시지 채널 또는 채널 어댑터를 연결하는 비교적 간단한 끝점입니다.

## **Configuring a Bridge with Java Configuration**

다음 예에서는 `@BridgeFrom` annotation을 사용하여 Java에서 브리지를 구성하는 방법을 보여줍니다.

```java
@Bean
public PollableChannel polled() {
    return new QueueChannel();
}

@Bean
@BridgeFrom(value = "polled", poller = @Poller(fixedDelay = "5000", maxMessagesPerPoll = "10"))
public SubscribableChannel direct() {
    return new DirectChannel();
}
```

다음 예는 `@BridgeTo` annotation을 사용하여 Java에서 브리지를 구성하는 방법을 보여줍니다.

```java
@Bean
@BridgeTo(value = "direct", poller = @Poller(fixedDelay = "5000", maxMessagesPerPoll = "10"))
public PollableChannel polled() {
    return new QueueChannel();
}

@Bean
public SubscribableChannel direct() {
    return new DirectChannel();
}
```

또는 다음 예제와 같이 `BridgeHandler`를 사용할 수 있습니다.

```java
@Bean
@ServiceActivator(inputChannel = "polled",
        poller = @Poller(fixedRate = "5000", maxMessagesPerPoll = "10"))
public BridgeHandler bridge() {
    BridgeHandler bridge = new BridgeHandler();
    bridge.setOutputChannelName("direct");
    return bridge;
}
```

## **Configuring a Bridge with the Java DSL**

다음 예제와 같이 JDSL(Java Domain Specific Language)을 사용하여 브리지를 구성할 수 있습니다.

```java
@Bean
public IntegrationFlow bridgeFlow() {
    return IntegrationFlow.from("polled")
            .bridge(e -> e.poller(Pollers.fixedDelay(5000).maxMessagesPerPoll(10)))
            .channel("direct")
            .get();
}
```
