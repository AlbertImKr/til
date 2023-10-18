# Error Channel

## Error Channel의 역할

### **장애 격리**:&#x20;

* 오류가 발생했을 때 전체 시스템이 중단되는 것을 방지하고, 문제가 있는 메시지만 분리하여 처리합니다. 이로 인해 전체 서비스의 가용성과 안정성이 향상됩니다.

### **진단 및 디버깅**:&#x20;

* 오류 토픽에 전송된 메시지를 분석하여 어떤 문제가 발생했는지 쉽게 파악할 수 있습니다. 오류 메타데이터(예: 오류 원인, 스택 트레이스)와 함께 저장되기 때문에 문제의 원인을 찾는 데 도움이 됩니다.

### **재처리**:&#x20;

* 오류의 원인을 해결한 후 오류 토픽에 있는 메시지를 다시 처리할 수 있습니다. 이를 통해 일시적인 오류로 인해 실패한 메시지도 나중에 성공적으로 처리될 수 있습니다.

### **모니터링 및 알림**:&#x20;

* Error Channel을 모니터링하여 오류 발생 비율이나 특정 임계값을 초과하는 경우 알림을 받을 수 있습니다. 이를 통해 시스템의 건강 상태를 지속적으로 확인하고 필요한 조치를 취할 수 있습니다.

### **시스템 복원력 향상**:&#x20;

* Error Channel을 활용하면 시스템은 예상치 못한 오류에 대해 더욱 탄력적으로 대응할 수 있습니다. 오류 발생 시 해당 메시지만 따로 처리하고, 정상 메시지는 계속 처리될 수 있기 때문입니다

## When user Error Channel ?

### 재처리가 필요한 경우

#### **예시: 외부 결제 서비스 호출**

* E-commerce 시스템에서는 주문 메시지를 처리할 때 외부의 결제 서비스를 호출합니다. 이 외부 서비스는 때때로 일시적인 오류를 반환할 수 있습니다.

```java
public class TemporaryPaymentException extends RuntimeException {
    public TemporaryPaymentException(String message) {
        super(message);
    }
}

@KafkaListener(topics = "order-topic")
public void processOrder(String order) {
    try {
        // ... 주문 로직 ...
        paymentService.charge(order);
    } catch (TemporaryPaymentException e) {
        // 일시적인 결제 오류: Error Channel로 전송하여 나중에 재처리
        errorKafkaTemplate.send("payment-retry-topic", order);
    }
}
```

### 분석이 필요한 경우

#### **예시: 유효하지 않은 주문 형식**

* 주문 메시지의 형식이 유효하지 않거나 예상치 못한 필드를 포함하고 있는 경우.

```java
public class InvalidOrderFormatException extends RuntimeException {
    public InvalidOrderFormatException(String message) {
        super(message);
    }
}

@KafkaListener(topics = "order-topic")
public void processOrder(String order) {
    try {
        // ... 주문 로직 ...
        orderValidator.validate(order);
    } catch (InvalidOrderFormatException e) {
        // 유효하지 않은 주문 형식: Error Channel로 전송하여 분석을 위한 저장
        errorKafkaTemplate.send("invalid-order-format-topic", order);
    }
}

```

### 특별한 대응이 필요한 경우

#### **예시: 큰 금액의 주문 거절**

* 상점은 일정 금액 이상의 주문에 대해 추가 검토가 필요합니다. 이런 주문들은 자동으로 거절되고 관리자에게 알림이 전송됩니다.

```java
public class LargeOrderException extends RuntimeException {
    public LargeOrderException(String message) {
        super(message);
    }
}

@KafkaListener(topics = "order-topic")
public void processOrder(String order) {
    try {
        // ... 주문 로직 ...
        if (orderService.isLargeOrder(order)) {
            throw new LargeOrderException("Order amount exceeds the limit");
        }
    } catch (LargeOrderException e) {
        // 큰 금액의 주문 거절: Error Channel로 전송하고 관리자에게 알림
        errorKafkaTemplate.send("large-order-topic", order);
        notificationService.notifyAdmin(e.getMessage());
    }
}

```

## 재처리 방법

### **수동 재처리**:&#x20;

* 관리자 또는 연산자가 특정 툴 또는 인터페이스를 사용하여 오류 메시지를 검토하고 수동으로 재처리를 결정할 수 있습니다.

### **자동 재시도**:&#x20;

* Kafka 같은 메시징 시스템은 메시지 소비 실패시 재시도 메커니즘을 자체적으로 제공할 수 있습니다. 이러한 설정은 Kafka consumer의 설정을 통해 조절할 수 있습니다. 재시도 횟수, 재시도 간격 등을 설정하여 자동으로 메시지를 재처리하도록 할 수 있습니다.

### **스케쥴링된 재처리**:&#x20;

* "payment-retry-topic"와 같은 특정 재시도 토픽에 메시지를 보내는 것 외에도, 스케쥴링된 작업(예: Spring Scheduled Task, Quartz Scheduler 등)을 사용하여 정기적으로 이 토픽을 폴링하고 재처리할 메시지가 있는지 확인하고 처리할 수 있습니다.

### **Dead Letter Queue (DLQ)**:&#x20;

* 재시도 횟수가 특정 횟수를 초과할 경우 메시지를 Dead Letter Queue(DLQ)라는 별도의 토픽으로 전송할 수 있습니다. DLQ에 있는 메시지는 수동으로 검토되고, 문제를 해결한 후 재처리될 수 있습니다.
