# Poller

## **Polling Consumer**

메시지 끝점(채널 어댑터)이 채널에 연결되고 인스턴스화되면 다음 인스턴스 중 하나를 생성합니다

* `PollingConsumer`
  * `PollableChannel` 인터페이스(예: QueueChannel)를 구현하는 채널에 연결된 채널 어댑터는 PollingConsumer의 인스턴스를 생성합니다.
* `EventDrivenConsumer`
  * `SubscribableChannel` 인터페이스를 구현하는 채널에 연결된 채널 어댑터는 EventDrivenConsumer 인스턴스를 생성합니다.

## **Pollable Message Source**

인바운드 채널 어댑터가 사용되는 경우 이러한 어댑터는 종종 SourcePollingChannelAdapter로 래핑됩니다.

예를 들어, 원격 FTP 서버 위치에서 메시지를 검색할 때 **FTP 인바운드 채널 어댑터**에 설명된 어댑터는 메시지를 주기적으로 검색하도록 폴러로 구성됩니다

구성 요소가 폴러로 구성된 경우 결과 인스턴스는 다음 유형 중 하나입니다.

* `PollingConsumer`
* `SourcePollingChannelAdapter`

이는 폴러가 인바운드 및 아웃바운드 메시징 시나리오 모두에서 사용됨을 의미합니다. 폴러가 사용되는 몇 가지 사용 사례는 다음과 같습니다.

* FTP 서버, 데이터베이스 및 웹 서비스와 같은 특정 외부 시스템 폴링
* 내부(폴링 가능) 메시지 채널 폴링
* 내부 서비스 폴링(예: Java 클래스에서 반복적으로 실행되는 메서드)

## **Deferred Acknowledgment Pollable Message Source**

버전 5.0.1부터 특정 모듈은 다운스트림 흐름이 완료될 때까지(또는 메시지를 다른 스레드로 전달) **연기 승인**을 지원하는 MessageSource 구현을 제공합니다. 이것은 현재 **AmqpMessageSource** 및 **KafkaMessageSource**로 **제한**됩니다

이러한 메시지 소스를 사용하면 IntegrationMessageHeaderAccessor.ACKNOWLEDGMENT\_CALLBACK 헤더(MessageHeaderAccessor API 참조)가 메시지에 추가됩니다.

폴링 가능한 메시지 소스와 함께 사용하는 경우 헤더 값은 다음 예제와 같이 AcknowledgementCallback의 인스턴스입니다.

```java
@FunctionalInterface
public interface AcknowledgmentCallback {

    void acknowledge(Status status);

    boolean isAcknowledged();

    void noAutoAck();

    default boolean isAutoAck();

    enum Status {

        /**
         * Mark the message as accepted.
         */
        ACCEPT,

        /**
         * Mark the message as rejected.
         */
        REJECT,

        /**
         * Reject the message and requeue so that it will be redelivered.
         */
        REQUEUE

    }

}
```

## **Conditional Pollers for Message Sources**

### "Smart" Polling

버전 5.3은 ReceiveMessageAdvice 인터페이스를 도입했습니다. 이 인터페이스를 구현하는 어드바이스 체인의 모든 어드바이스 객체는 receive() 작업에만 적용됩니다.`(MessageSource.receive() 및 PollableChannel.receive(timeout))` .**`SourcePollingChannelAdapter`** 또는 **`PollingConsumer`**에만 적용할 수 있습니다. 이러한 클래스는 다음 메서드를 구현합니다.

* beforeReceive(Object source) 이 메서드는 Object.receive() 메서드 전에 호출됩니다. 이를 통해 소스를 검사하고 재구성할 수 있습니다. false를 반환하면 이 poll가 취소됩니다(앞서 언급한 PollSkipAdvice와 유사).
* `Message afterReceive(Message result, Object source)` 이 메소드는 receive() 메소드 이후에 호출됩니다.&#x20;

{% hint style="danger" %}
**Thread safety**

Advice가 소스를 변경하는 경우 TaskExecutor로 폴러를 구성하면 안 됩니다. Advice가 소스를 변경하는 경우 이러한 변경은 스레드로부터 안전하지 않으며 특히 고주파 폴러에서 예기치 않은 결과를 초래할 수 있습니다. 폴링 결과를 동시에 처리해야 하는 경우 폴러에 실행기를 추가하는 대신 다운스트림 ExecutorChannel을 사용하는 것이 좋습니다.
{% endhint %}

{% hint style="danger" %}
**Advice Chain Ordering**

초기화 중에 어드바이스 체인이 처리되는 방식을 이해해야 합니다. ReceiveMessageAdvice를 구현하지 않는 어드바이스 개체는 전체 폴링 프로세스에 적용되며 모두 순서대로 ReceiveMessageAdvice보다 먼저 호출됩니다. 그러고 ReceiveMessageAdvice 객체가 소스 receive() 메서드 주변에서 순서대로 호출됩니다.
{% endhint %}

### **CompoundTriggerAdvice**

트리거의 기본 트리거는 CronTrigger일 수 있습니다. 어드바이스가 수신된 메시지가 없음을 감지하면 두 번째 트리거를 `CompoundTrigger`에 추가합니다.`CompoundTrigger` 인스턴스의 `nextExecutionTime` 메서드가 호출되면 보조 트리거(있는 경우)에 위임합니다. 그렇지 않으면 기본 트리거에 위임합니다.



## Reference

{% embed url="https://docs.spring.io/spring-integration/docs/current/reference/html/core.html#polling-consumer" %}
