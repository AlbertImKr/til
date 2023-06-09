# Builder Pattern

제품을 여러 단계로 나눠서 만들도록 제품 생상 단계를 캡

<figure><img src="../../../.gitbook/assets/image (1) (2) (1).png" alt=""><figcaption><p>Wiki</p></figcaption></figure>

## Why?

Builder 디자인 패턴은 다음과 같은 문제를 해결합니다.

* 클래스(동일한 구성 프로세스)가 복잡한 객체의 다른 표현을 어떻게 생성할 수 있습니까?&#x20;
* 복잡한 객체 생성을 포함하는 클래스를 어떻게 단순화할 수 있습니까?

## How?

빌더 디자인 패턴은 이러한 문제를 해결하는 방법을 설명합니다.

* 별도의 Builder 개체에서 복잡한 객체의 일부를 만들고 조립하는 것을 캡슐화합니다.&#x20;
* 클래스는 객체를 직접 생성하는 대신 객체 생성을 Builder 객체에 위임합니다.

## Advantages

* 제품의 내부 표현을 변경할 수 있습니다.&#x20;
* 생성 및 표현을 위해 코드를 캡슐화합니다.&#x20;
* 구성 프로세스의 단계를 제어할 수 있습니다.
* 여러 단계와 다양한 절차를 거쳐 객체를 만들 수 있습니다.
* 제품의 내부 구조를 클라이언트로부터 보호할 수 있습니다.

## Disadvantages

* 각 제품 유형에 대해 고유한 ConcreteBuilder를 생성해야 합니다.&#x20;
* 빌더 클래스는 변경 가능해야 합니다.&#x20;
* 의존성 주입을 방해하거나 복잡하게 만들 수 있음



