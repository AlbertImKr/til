# HTTP

## URL과 URN

* URL(Uniform Resource Locator)
  * 웹 주소
* URN(Uniform Resource Name)
  * 특정 네임스페이스에서 이름으로 리소스를 식별하는 URI

### URI(Uniform Resource Identifier) 구문

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

* 인터넷
  * 인터넷 프로토콜(IP, Internet Protocol)을 공유해 메시지를 라우팅하는 식으로 연결된 공용 컴퓨터의 모음이다.
  * 월드 와이드 웹, 이메일, 파일 공유, 인터넷 전화 등 포함
* HTTP
  * 웹 브라우저가 웹 페이지를 요청하는 방식이다.
  * URI 및 Hypertext Markup Language와 함께 (팀 버너스 리Tim Berners-Lee)가 웹 발명했을 때 정의한 세 가지 주요 기술 중 하나다.

## 작동 순서

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

1. IP주소를 전화번호라고 생각하면 DNS(Domain Name System)는 전화번호부다. - DNS는 가장 가까운 서버의 IP 주소를 제공한다.
2. 웹 브라우저가 컴퓨터에 이 주소로 표준 웹포트(포트 80) 또는 표준 보안 웹 포트(포트 443)에 IP를 통한 TCP(Transmission Control Protocol)연결을 요청한다.
3. 브라우저가 웹 서버와 연결을 맺고 있으면 웹 사이트를 요청하기 시작할 수 있다.
4. 서버는 요청받은 URL에 응답한다.
5. 웹 브라우저가 반환된 응답을 처리한다. - HTML 코드 해석을 시작하고 메모리에 해당 페이지의 내부 표현인 문서 객체 모델(DOM,Document Objet Model)을 구축한다.
6. 웹 프라우저가 추가로 필요한 리소스를 요청한다.
7. 브라우저가 중요한 리소스를 충분히 얻으면 화면에 페이지를 렌더링하기 시작한다.
8. 페이지를 처음 표시한 후 웹 브라우저는 백그라운드에서 페이지에 필요한 다른 리소스를 계속 다운로드하고 리소스를 처리하는 대로 페이지를 업데이트한다.
9. 페이지가 완전히 로드되면 브라우저는 로딩 아이콘을 멈추고, 자바스크립트 코드에서 페이지가 어떤 동작을 수행할 준비가 됐다는 표시로 사용 할 수 있는 OnLoad 자바스크립트 이벤트를 발생시킨다.
10. 이 시점에서 페이지는 완전히 로드됐지만 브라우저는 요청 전송을 머추지 았다,

## OSI 7 계층

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>OSI 7 계층</p></figcaption></figure>

## HTTP 1.0

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## HTTP/1.1

* 호스트 헤더가 필수적인 값이 됐으며 지속적인 연결이 추가됐다는 점이 HTTP/1.0 구문과의 두드러지는 차이점이 있다.
* 필수적인 호스트 헤더
* 지속적인 연결(keep-alive)
  * Connection HTTP 해더로 해결
* 기타 새로운 기능
  * HTTP/1.0에 정의된 GET,POST,HEAD 메서드에 추가된 새로운 메서드는 PUT,OPTIONS와 잘 사용되지는 않는 CONNECT, TRACE, DELETE다
  * 개선된 캐싱 방법을 도입해 서버가 리소스(CSS 파일과 같은)를 브라우저의 캐시에 저장해서 필요하면 나중에 재사용할 수 있도록 클라이언트에 지시할 수 있다. Cache-Control HTTP 헤더는 HTTP/1.1에 HTTP/1.0의 Expires 헤더보다 많은 옵션을 도입했다.
  * HTTP가 상태 없는 프로토콜에서 상태를 가질 수 있게 하는 HTTP 쿠키를 도입했다.
  * HTTP 응답에서의 문자 집합(character set)과 언어를 도입했다.
  * 프록시 지원 기능을 도입했다.
  * 인증 기능을 도입했다.
  * 새로운 상태 코드를 도입했다.
  * 후행 헤더를 도입했다.

### HTTP/1.1의 근본적인 성능 문제

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* 대기 시간이 길다.
  * 대기시간은 단일 메시지를 서버에 전송하는 데 걸리는 시간을 측정하는 반면, 대역폭은 사용자가 이 메시지에서 다운로드 할 수 있는 양을 측정한다

### 파이프라이닝

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

* 여전히 파이프라이닝에서는 응답이 요청의 순서대로 반환돼야 할 필요가 있다
  * HOL(Head-Of-Line) 블로킹
* HTTP/1.1 성능 문제의 우회적 해결 방법
  * HTTP/1.1은 동기적이다.(synchronous)
  * 두 부류
    * 여러 HTTP 연결을 사용한다.
    * 적지만 잠재적으로 큰 HTTP 요청을 만든다.
      * 사용자가 리소스를 최적의 방식으로 요청하고(예를 들어 중요한 CSS를 먼저 요청), 다운로드하는 양을 줄이고(압축과 반응형 이미지), 브라우저의 작업을 줄이는(더욱 효율적인 CSS와 자바스크립트)것을 보장하는 방식을 수반한다.

### 여러 HTTP 연결 사용

* HTTP/1.1의 블로킹 문제를 회피하는 가장 쉬운 방법 중 하나는 여러 개의 연결을 맺어 병렬로 여러 개의 HTTP 요청이 끊임없이 동작하게 하는 것이다.
* 도메인 샤딩(domain sharding)
  * 병렬화 정도를 증가시키는 것 외에도 쿠키의 HTTP 헤더를 감소시키는 등의 역할을 하기 때문에 유요할 수 있다.
* 주요 이슈는 하부 TCP 프로토콜에서의 커다란 비효율성이다.
  * TCP는 고유 시퀸스 번호가 있는 패킷을 전송하고 누락된 시퀜스 번호를 검색해서 도중에 손실된 패킷을 다시 요청하는 것이 보장된 프로토콜이다.

### TCP는 3 방향 핸드셰이크(three-way handshake)

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

1. 클라이언트는 서버에게 시퀸스 번호를 알려주는 동기화(SYN) 메시지를 전송한다. 클라이언트는 이 요청부터 모든 미래의 TCP 패킷이 이 시퀸스번호에 기반을 둘 것으로 기대할 것이다.
2. 서버는 클라이언트가 보낸 시퀸스 번호를 받았다고 알리고 서버 자체의 동기화 요청을 전송해 클라이언트에게 서버의 메시지에 사용할 시퀸스 번호를 전한다. 두 메시지는 하나의 SYN-ACK 메시지로 결합된다.
3. 마지막으로 클라이언트는 ACK 메시지로 서버의 시퀸스 번호를 받았다고 알린다.

#### 정체 창 (CWND, congestion window)

* 시간이 지나는 데 따라 연결이 패킷 손실 없이 더 큰 크기를 처리할 수 있는 것으로 보이면서 정차 증가한다.
* TCP 정체 창의 크기는 TCP 느린 시작 알로리즘을 제어한다.
* TCP는 네트워크에 과부하를 주지 않으려고 하는 보장된 프로토콜이다.
* 여러 개의 연결을 맺는 것은 HTTP 수준에서 괜찮은 최적화이더라도 TCP와 HTTPS 수준에서는 비효율적이다.

### 요청 수 줄이기

* 요청 수 줄이기 기법
  * 불필요한 요청을 줄이거나(브라우저에 자산을 캐시하는 등) 적은 수의 HTTP 요청을 동일한 양의 데이터를 요청하는 것 등이 포함된다.
  * 전자의 방법은 HTTP 캐싱 헤더 사용과 연관돼 있고
  * 후자의 방법은 자산을 결합된 파일로 묶는(bundle) 일이 수반된다.
* 스프라이팅(spriting)
  * 이미지에 대한 번들링 기법
  * 설정하는 데 노력이 필요하다.
* 리소스를 다른 파일에 인라인시키는 것도 포함된다.
  * 단점은 복잡성
  * 낭비
  * 캐싱
* 이 기법은 네트워크 계층과 처리의 측면에서 모두 비효율적이다.

## HTTPS

* HTTP가 일반 텍스트이기 때문에 도중에 메시지를 가로채고,읽고,고쳐 쓰기까지 할 수 있다.
* 전송 중의 메시지를 전송 계층 보안(TLS, Transport Layer Security) 프로토콜을 사용해 암호화하는 HTTP의 보안 버전이다.
  * TLS는 종종 이전 이름인 보안 소켓 계층(SSL,Secure Sockets Layer)
* 추가한 세가지 개념
  * 암호화: 메시지는 전송 중에 제3자에게 읽힐 수 없다.
  * 무결성: 암호화된 메시지가 디지털 서명되고 서명이 복호화되기 전에 암호학적으로 검증되기 때문에 메시지는 전송 중에 변경되지 않는다.
  * 인증: 서버는 클라이언트가 메시지를 주고받으려던 바로 그 서버다.

### HTTPS 핸드세이크

* HTTPS를 사용하는 것은 HTTP/1이는 HTTP/2든 간에 표준 HTTP 연결을 암호화하는 데 SSL/TLS를 사용한다는 뜻이다.
* 공개-비공개키 암호화는 비대칭형 암호화라고 알려져 있는데, 메시지를 암호화하고 복호화하는 데 다른 키를 사용하기 때문이다. 이러한 유형의 암호화는 이전에 한 번도 연결한 적이 없는 서버와 보안 통신을 하는데 필요하지만 느리므로 나머지 연결을 암호화하는 데 사용할 대칭형 암호화키를 합의하는 데 사용한다. 이 합의는 TLS 핸드세이크 도중에 일어나는데, 핸드세이크는 연결의 시작을 일어난다.

<figure><img src="../../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

### ALPN

* ALPN는 ClientHello 메시지에 추가적인 확장 기능을 더해 클라이언트가 애플리케이션 프로토콜 지원을 알릴 수 있게 했고,ServerHello 메시지에도 추가해 서버가 HTTPS 협상 이후에 어떤 애플리케이션 프로토콜을 사용할지 확정할 수 있게 한다.

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

* ALPN은 간단하고 더 이상의 어떤 라운드 드립, 리디렉션, 그 외의 업그레이드 지연을 추가하지 않고도 기존 HTTPS 협상 메시지에 대해 HTTP/2 사용 여부를 합의하는 데 사용할 수 있다.
* ALPN의 유일한 문제
  * 비교적 새로운 프로토콜이기 때문에 보편적으로 지원되지 않으며, 특히 오래된 TLS 라이브러리가 흔한 서버 측에서 지원이 잘되지 않는다는 점이다.
  * ALPN인 지원되지 않으면 서버는 보통 클라이언트가 HTTP/2를 지원하지 않는다고 가정하며 HTTP/1.1을 사용한다.

### NPN

* ALPN의 전신은 NPN(Next Protocol Negotiation)
* NPN에서는 클라이언트가 사용되는 프로토클을 결정하지만 ALPN에서는 서버가 정한다는 점이 주된 차이점이다.

<figure><img src="../../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

## HTTP/1.1 과 HTTP/2

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption></figcaption></figure>

## HTTP/2 푸시란

### HTTP/2 서버 푸시란?

* HTTP/2 서버 푸시(이후부터 HTTP/2 푸시)를 사용하면 클라이언트가 요청하지 않은 추가 리소스를 서버가 클라이언트로 보낼 수 있다.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

* 라운드 트립 지연은 HTML 페이지에 스타일시트를 \<style> 태그로 인라이닝하거나 \<script> 태그를 사용해 자바스크립트에도 비슷한 작업을 하는 등의 성능 최적화로 이어진다.
  * 중요 리소스를 인라이닝함으로써 브라우저가 첫 페이지를 다운로드하자마자 추가적인 중요 리소스를 기다리는 대신 첫 렌더링을 시작하고 해석할 수 있다.
  * 리소스를 인라이닝하는 데는 몇 가지 단점이 있다.
    * CSS 리소스에서 필요한 만큼의 중요한 스타일만 빼내 HTML 파일에 끼워 넣는 것은 이 작업을 돕는 도구가 있음에도 불구하고 복잡하다. 복잡할 뿐만 아니라 이런 절차는 낭비다. 중요한 CSS는 캐시돼 다른 페이지에 재사용될 수 있는 CSS 파일 하나에 저장되기보다는 웹 사이트의 모든 페이지에서 재사용될 수 있는 CSS 파일 하나에 저장되기보다는 웹 사이트의 모든 페이지에 중복돼 저장된다. 더 나쁜 점은 인라인된 중요 CSS가 보통 나중에 로드되는 주 CSS 파일에 여저히 포함된다는 점이다. 페이지마다 중복될 뿐만 아니라 페이지 안에서도 중복된다.
    * 중요하지 않은 CSS파일도 모두 로드하는 자바스크립트르 사용해야 하는 요구 사항을 포함한다. 게다가 이 중요한 CSS파일 중 어떤 것이든 변경하려 한다면 하나의 공통 CSS파일만 갱신하는 대신 모든 페이지를 변경해야 한다

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* HTTP/2 푸시는 HTTP가 항상 ‘하나의 요청 = 하나의 응답’이라는 패러다임을 깨뜨린다. HTTP/2 푸시를 사용하면 서버가 하나의 요청에 대해 여러 응답으로 답할 수 있다.

<figure><img src="../../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

* HTTP 푸시가 웹소켓이나 SSE를 대체하는가?
  * HTTP/2는 엄밀하게 양방향은 아니다.
  * 모든 것은 여전히 클라이언트 측의 요청에서 시작된다. 푸시된 리소스는 첫 요청에 대한 응답에 추가 응답을 만들어진다.
  * 청 요청이 종료되면 스트림이 닫히고, 다른 클라이언트 요청이 없다면 더 이상 어떤 리소스도 푸시될 수 없다.

### 링크 헤더를 사용해 다운스트림 시스템에서 푸시

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* 이 그림은 HTTP/2 연결을 통해 아무것도 송신되거나 수신되지 않은 큰 공백을 보여주는데, 이는 낭비며 HTTP/2가 해결하려고 했던 HOL(Head-Of-Line) 블로킹 문제를 연상시킨다.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* H2PushResource와 같은 웹서버의 이른 푸시 명령을 사용하는 데 따른 단점은 애플리케이션이 이러한 푸시를 개시하게 할 수 없으며, 애플리케이션이 푸시여부를 결정하기에 최적의 위치일 것이라는 점이다. 이 상황을 해결하고자 새로운 HTTP 상태 코드(103 Early Hint)를 사용해서 프리코드 HTTP 링크 헤더를 통한 리소스 요구 사항을 더 이르게 암시할 수 있다.
  * 이 코드를 사용하면 헤더만 있는 이른 응답을 보낼 수 있으며 , 드다음에는 표준 200 등답 코드가 따라온다.

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 브라우저에서 푸시 캐시 동작 방식

* 푸시된 리소스는 브라우저가 리소스를 요청하기를 기다리는 별도 메모리에 보관되는데, 여기서 리소스가 페이지로 로드된다.
* 캐시 헤더가 설정된 경우 보통 때와 같이 나중에 재사용할 용도로 브라우적의 HTTP 캐시도 저장도니다.
  * 눈에 띄는 예외 사항은 크로미움 기반 브라우저(Chromium-based browers)는 신뢰하지 않는 인증서에 대한 리소스를 캐시하지 않는다는 점이다.
  * 인증서 오류를 찾아낸 경우에도 여전히 캐시가 사용되지 않는다.

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

* 이미지 캐시는 수명이 짧은 메모리상의 캐시로, 페이지에 대한 캐시다.
  * 예를 들면 브라우저가 페이지에서 두번 참조하는 이미지를 두번 가져오지 않고 하고자 사용된다. 사용자가 페이지에서 다른 곳으로 브라우징하면 캐시는 없어진다.
* 프리로드 캐시는 또 하나의 수명이 짧은 메모리상의 캐시로, 프리로드된 리소스를 저장하는 데 사용된다. 다시 한 번 이 캐시는 페이지에 특정적이다. 다른 페이지를 위해 프리로드하지 말라, 사용되지 않을 것이기 때문이다.
* 서비스 워커는 웹 페이지와 별개로 실행되고 웹 페이지와 웹 사이트의 중개가 역할을 하는 상당히 새로운 백그라운드 애플리케이셔이다. 서비스 위커를 사용하면 웹 사이트가 더욱 원시 애플리케이션이다. 서비스 워커를 사용하면 웹 사이트가 더욱 원시 애플리케이션처럼 동작하게 할 수 있는데, 예를 들어 네트워크 연결을 잃었을 때조차 그렇다. 서비스 워커는 도메인에 연결된 고유한 캐시를 갖는다.
* HTTP 캐시는 개발자 대부분이 아는 중요 캐시며, 디스크 기반의 지속성 캐시로 브라우저에서 공유되고,모든 도메인에 대해 사용되는 제한된 크기를 갖는다.
* HTTP/2 푸시 캐시는 수명이 짧은 메모리상의 캐시로, 연결에 매여 있으며 마지막으로 검사된다.

## Reference

> 책: HTTP/2 in Action
