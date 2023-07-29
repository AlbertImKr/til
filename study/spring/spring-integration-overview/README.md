# Spring Integration Overview

Spring Integration은 [_Enterprise Integration Patterns_](https://en.wikipedia.org/wiki/Enterprise\_Integration\_Patterns)을 지원하기 위해 Spring 프로그래밍 모델의 확장이다. Spring 기반 애플리케이션 내에서 경량 **메시징**을 가능하게 하고 선언적 **어댑터**를 통해 **외부 시스템**과의 **통합**을 지원합니다.&#x20;

Spring Integration의 주요 **목표**는 유지 관리 및 테스트 가능한 코드를 생성하는 데 필수적인 **관심사의 분리**를 유지하면서 엔터프라이즈 통합 솔루션을 구축하기 위한 간단한 모델을 제공하는 것입니다.

## Goals and Principles

### golas

* 복잡한 엔티프라이즈 통합 솔루션을 구현하기 위한 간단한 모델을 제공한다
* Spring 기반 애플리케이션 내에서 비동기식 메시지 기반 동작 용이하게 한다
* 기존 Spring 사용자를 위한 직관적이고 점진적인 채택을 추진한다

### priciples

* 구성 요소는 모듈성과 테스트 가능성을 위해 느슨하게 결합되어야 합니다.
* 프레임워크는 비즈니스 로직과 통합 로직 간의 관심사 분리를 시행해야 합니다.
* 확장 지점은 본질적으로 추상적이어야 하지만 재사용 및 이식성을 위해서 잘 정의된 경계 내에 있어야 합니다.

## Main Components

메시징 시스템은 일반적으로 유사하게 추상적인 ``**`pipes-and-filters`** 모델을 따릅니다.  **filters**는 메시지를 생성하거나 소비할 수 있는 모든 구성 요소를 나타내고 **pipes**는 구성 요소 자체가 느슨하게 연결된 상태를 유지하도록 필터 간에 메시지를 전송합니다.

### Message

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Message</p></figcaption></figure>

**payload**는 모든 유형이 될 수 있으며 **headers**에는 ID, 타임스탬프, 상관 관계 ID 및 반환 주소와 같은 일반적으로 필요한 정보가 포함됩니다. **Headers**는 연결된 전송과 값을 주고 받는 데에도 사용됩니다.



**예**를 들어, 수신된 파일에서 메시지를 생성할 때 파일 이름은 다운스트림 구성 요소에서 액세스할 수 있도록 헤더에 저장될 수 있습니다. 마찬가지로 메시지 내용이 궁극적으로 아웃바운드 메일 어댑터에 의해 전송되는 경우 다양한 속성(받는 사람, 보낸 사람, 참조, 제목 등)이 업스트림 구성 요소에 의해 메시지 헤더 값으로 구성될 수 있습니다. 개발자는 임의의 키-값 쌍을 헤더에 저장할 수도 있습니다.

### Message Channel

메시지 채널은 **pipes-and-filters** 아키텍처의 **pipe**를 나타냅니다.&#x20;

메시지 채널은 메시징 구성 요소를 **분리**하고 메시지 **가로채기**와 **모니터링**을 위한 convenient **point**을 제공한다.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Message Channel</p></figcaption></figure>

메시지 채널은 **point-to-point** 또는 **publish-subscribe semantics**을 따를 수 있습니다.

#### Point-to-point channel

한 명의 소비자만 채널로 전송된 각 메시지를 받을 수 있습니다.

#### Publish-subscribe channels

채널의 모든 구독자에게 각 메시지를 뿌린다.&#x20;

### Message Endpoint

Spring Integration의 주요 목표 중 하나는 Inversion of Control을 통해 엔터프라이즈 통합 솔루션의 개발을 단순화하는 것입니다. 즉, 소비자와 생산자를 직접 구현할 필요가 없으며 메시지를 작성하고 메시지 채널에서 보내기 또는 받기 작업을 호출할 필요도 없습니다.



{% content-ref url="message-endpoints.md" %}
[message-endpoints.md](message-endpoints.md)
{% endcontent-ref %}

##

## Dependencies

```groovy
implementation 'org.springframework.boot:spring-boot-starter-integration'
implementation 'org.springframework.integration:spring-integration-http'
testImplementation 'org.springframework.integration:spring-integration-test'
```

## Reference

{% embed url="https://docs.spring.io/spring-integration/docs/current/reference/html/overview.html#overview" %}
