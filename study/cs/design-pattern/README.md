# Design Pattern

## **1. 전략 패턴**

* 알고리즘군을 정의하고 캡슐화해서 각각의 알고리즘군을 수정해서 쓸 수 있게 해 줍니다.
* 전략 패턴을 사용하면 클라이언트로부터 알고리즘을 분리해서 독립적으로 변경할 수 있습니다.

<figure><img src="https://blog.kakaocdn.net/dn/ogc3r/btr4v9mxUO1/K1Uuy8FWtvf6NmVR9Cgqo1/img.png" alt=""><figcaption></figcaption></figure>

## **2. 옵저버 패턴**

* 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체에게 연락이 가고 자동으로 내용이 갱신되는 방식으로 일대다(one-to-many)의존성을 정의합니다.

<figure><img src="https://blog.kakaocdn.net/dn/dOVYKq/btr4WGhRHvc/fUAnFY90YdBviOfRX4OV70/img.png" alt=""><figcaption></figcaption></figure>

## **3. 데코레이터 패턴**

* 객체에 추가 요소를 동적으로 더할 수 있습니다.
* 데코레이터를 사용하면 서브클래스를 만들 때보다 휠씬 유연하게 기능을 확자할 수 있습니다.

<figure><img src="https://blog.kakaocdn.net/dn/vHuXZ/btr4LG3Tmxx/AZh8qsjZIrNSmcVwLkg8R0/img.png" alt=""><figcaption></figcaption></figure>

## **4. 팩토리 메소드 패턴**

* 객체를 생성할 때 필요한 인터페이스를 만듭니다. 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정합니다. 팩토리 메소드 패턴을 사용하면 클래스 인스턴스 만드는 일을 서브클래스에서 맡기게 됩니다

<figure><img src="https://blog.kakaocdn.net/dn/cKISFa/btr4LIglnDH/nDPVmcKxMDhfGZtQyKQUPk/img.png" alt=""><figcaption></figcaption></figure>

## **5. 추상 팩토리 패턴**

구상 클래스에 의존하지 않고도 서로 연관되거나 의존적인 객체로 이루어진 제품군을 생상하느 인터페이스를 제공합니다. 구상 클래스는 서브클래스에서 만듭니다.

<figure><img src="https://blog.kakaocdn.net/dn/CYSsf/btr4RzwzS1m/bIppbk8sAJy14r3F4FymNK/img.png" alt=""><figcaption></figcaption></figure>

## **6. 싱글턴 패턴**

* 클래스 인스턴스를 하나만 만들고, 그 인스턴스로의 전역 접근을 제공합니다.

<figure><img src="https://blog.kakaocdn.net/dn/Lc4kA/btr4RAbbkyE/wKcdM1OcwWxCqBsfeArQUk/img.png" alt=""><figcaption></figcaption></figure>

## **7. 커맨드 패턴**

* 사용하면 요청 내역을 객체로 캡슐화해서 객체를 서로 다른 요청 내역에 따라 매개변수화할 수 있습니다. 이러한 요청을 큐에 저장하거나 로그로 기록하거나 작업 취소 기능을 사용할 수 있습니다.

<figure><img src="https://blog.kakaocdn.net/dn/ErgO7/btr4JLxwqah/Vt3JxkgFKweHSmw6ZuS72k/img.png" alt=""><figcaption></figcaption></figure>

## **8. 어댑터 패턴**

* 특정 클래스 인터페이스를 클라이언트에서 요구하는 다른 인터페이스로 변환합니다.
* 인터페이스가 호환되지 않아 같이 쓸 수 없었던 클래스를 사용할 수 있게 도와줍니다.

<figure><img src="https://blog.kakaocdn.net/dn/UKgu4/btr4yk8C2OK/196XMaNa6coc8zv0Ay4SHk/img.png" alt=""><figcaption></figcaption></figure>

## **9. 퍼사드 패턴**

* 클라이언트와 내부 시스템 간의 상호작용을 간소화하고, 클라이언트가 내부 시스템의 복잡한 동작을 알 필요 없이 단일 인터페이스를 통해 상호작용할 수 있게 해줍니다.



<figure><img src="https://blog.kakaocdn.net/dn/cSDL3j/btr4xjIUKSH/7maa5uE7ItlvltIFdjjMK1/img.png" alt=""><figcaption></figcaption></figure>

## **10. 템플릿 메서드 패턴(Template Method Pattern)**

* 특정한 알고리즘의 구조를 정의하면서 각 단계의 구현을 서브 클래스에게 맡기는 방식으로, 알고리즘의 구조는 공통으로 유지되지만 각 단계의 구현은 서브 클래스에 따라 다를 수 있도록 합니다.

<figure><img src="https://blog.kakaocdn.net/dn/bpN4OW/btr4xLywYho/GtTXQ4WbI1aqq5QmEmM00k/img.png" alt=""><figcaption></figcaption></figure>

## **11. 반복자 패턴(Iterator Pattern)**

* 내부 구현 방법을 외부로 노출하지 않으면서 집합체에 있는 모든 항목에 일일이 접근할 수 있습니다.
* 또한 각 항목에 일일이 접근할 수 있게 해 주는 기능을 집합체가 아닌 반복자 객체가 책임진다는 장점도 있습니다.
* 그러면 집합체 인터페이스와 구현이 간단해지고, 각자에게 중요한 일만을 처리할 수 있게 됩니다.

<figure><img src="https://blog.kakaocdn.net/dn/yC067/btr4w6bPY1x/shxxg1FndwmAuh9kbrSwX1/img.png" alt=""><figcaption></figcaption></figure>

## **12. 컴포지트 패턴**

* 객체를 트리구조로 구성해서 부분-전체 계층구조를 구합니다. 컴포지트 패턴을 사용하면 클라이언트에게 개별 객체와 복합 객체를 똑같은 방법으로 다룰 수 있습니다.
* 컴포지트 패턴을 사용하면 객체의 구성과 객별 객체를 노드로 가지는 트리 형태의 객체 구조를 만들 수 있습니다.
* 이런 복합 구조(composite Structure)를 사용하면 복합 객체와 개별 객체를 대상으로 똑같은 작업을 적용할 수 있습니다.

<figure><img src="https://blog.kakaocdn.net/dn/W7oH4/btr4DtD2vcs/op7Q4dynXe8zRC0ReoBZz1/img.png" alt=""><figcaption></figcaption></figure>

## **13. 상태 패턴(State Pattern)**

* 객체의 내부 상태가 바뀜에 따라서 객체의 행동을 바꿀 수 있습니다.
* 마치 객체가 바뀌는 것과 같은 결과를 얻을 수 있습니다

<figure><img src="https://blog.kakaocdn.net/dn/l25qK/btr4yj9I3Zk/Isu6kCZxAl4TqGlAZLUltk/img.png" alt=""><figcaption></figcaption></figure>

## **14. 프록시 패턴(Proxy Pattern)**

* 특정 객체로의 접근을 제어하는 대리인 (특정 객체를 댈변하는 객체)을 제공한다.

<figure><img src="https://blog.kakaocdn.net/dn/cELvrq/btr4LEZiryE/6WZpqygqPmxTjWBHvegXF1/img.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://blog.kakaocdn.net/dn/bjLfWk/btr4YtpbwlO/ezZRe8gTlFrNtUFUKoRag0/img.png" alt=""><figcaption></figcaption></figure>

\


**Comming Soon**

\-----

참고 :

책: 헤드 퍼스트 디자인 패턴(에릭 프리먼 , 엘리자베스 롭슨 , 케이시 시에라 , 버트 베이츠 저자(글) · 서환수 번역) 한빛미디어

웹: [https://www.wikipedia.org/﻿](https://www.wikipedia.org/)
