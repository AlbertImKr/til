---
description: Authentication
---

# 인증

인증이란?

* 애플리케이션이 이를 이용하려는 사람을 식별하는 프로세스를 말한다.

## Authentication 인터페이스

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Authentication 계약은 Principal 계약을 상속한다. Authentication에는 암호 같은 요구 사항이나 인증 요청에 대한 세부 정보를 더 추가할 수 있다. 이러한 세부 벙보 중 일부(예: 권한의 목록)는 스프링 시큐리티에 특화된 것이다.</p></figcaption></figure>

## AuthenticationProvide 메서드

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p>AuthenticationManager는 사용 가능한 인증 공급자 중 하나에 인증을 위임한다. AuthenticationProvider는 주어진 인증 유형을 지원하지 않거나 객체 유형은 지원하지만 해당 특정 객체를 인증하는 방법을 모를 수 있다. 인증을 평가한 후 요청이 올바른지 판단할 수 있는 AuthenticationProvider가 AuthenticationManager에 응답한다.</p></figcaption></figure>

### authenticate() 구현 방법

* 인증이 실패하면 메서드는 AuthenticationException을 투척해야 한다.
* 메서드가 현재 AuthenticationProvider 구현에서 지원되지 않는 인증 객체를 받으면 null을 반환해야 한다. 이렇게 하면 HTTP 필터 수준에서 분리된 여러 Authentication 형식을 사용할 가능성이 생긴다.&#x20;
* 메서드는 완전히 인증된 객체를 나타내는 Authentication 인스턴스를 반환해야 한다. 이 인스턴스에 대해 isAuthenticated() 메서드는 true를 반환하며, 여기에는 인증된 엔티티의 모든 필수 세부 정보가 포함된다. 또한 일반적으로 애플리케이션은 이 인스턴스에서 암호화 같은 민감한 데이터를 제거해야 한다. 인증한 후에는 암호가 더는 필요 없으며 이러한 세부 정보를 그냥 두면 원치 않게 유출될 우려가 있다.

### suports(Class\<?> authentication)

* 현재 AuthenticationProvider가 Authentication 객체로 제공된 형식을 지원하면 ture를 반환하도록 구현한다

### 흐름

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption><p>AuthenticationProvider에 의해 구현된 맞춤형 인증 흐름</p></figcaption></figure>

## SecurityContext

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>인증이 성공한 후 인증 필터는 인증된 엔티티의 세부 정보를 보안 컨텍스트에 저장한다.</p></figcaption></figure>

### 관리하는 세가지 전략

* MODE\_THREDLOCAL
  * 각 스레드가 보안 컨텍스트에 각자의 세부 정보를 저장할 수 있게 해준다. 요청당 스레드 방식의 웹 애플리케이션에서는 각 요청이 개별 스레드를 가지므로 이는 일반적인  접근법이다.
* MODE\_INHERITABLETHREADLOCAL
  * MODE\_THREADLOCAL과 비슷하지만 비동기 메서드의 경우 보안 컨테스트를 다음 스레드를 복사하도록 스프링 시큐리티에 지시한다. 이 방식으로 `@Async` 메서드를 실행하는 새 스레드가 보안 컨텍스트를 상속하게 할  수 있다.
* MODE\_GLOBAL
  * 애플리케이션의 모든 스레드가 같은 보안 컨텍스트 인스턴스를 보게 한다

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption><p>각 요청이 자체 스레드를 가진다</p></figcaption></figure>

### DelegatingSecurityContextCallable

* 특정한 SecurityContextHolder 전략으로 자체 관리 스레드를 위한 해결책을 얻을 수 없으며 개발자가 보안 컨텍스트 전파를 해결해야 한다. 한 가지 해결책은 별도의 스레드에서 실행하고 싶은 작업을 DelegatingSecurityContextRunnable로 장식하는 것이다.
* DelegatingSecurityContextRunnable은 Runnable을 확장하며 반환 값이 없는 작업 실행 후 이용할 수 있다.
* 반환 값이 있는 작업에는 DelegatingSecurityContextCallable\<T>에 해당하는 Callble\<T> 대안을 이용할 수 있다.

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption><p>DelegatingSecurityContextCallable은 Callable 객체의 데코레이터로 디자인됐다</p></figcaption></figure>

### DelegatingSecurityContextExecutorService

* DelegatingSecurityContextExecutorService는 보안 컨텍스트 전파도 처리한다

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>DelegatingSecurityContextExecutorService는 ExecutorService를 작성해 작업하기 전에 보안 컨텍스트 세부 정보를 다음 스레드로 전파한다</p></figcaption></figure>

## HTTP Basic 인증

```http
GET /api/users HTTP/1.1
Host: example.com
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

* 인증 정보는 Authorization 헤더에 Basic 접두사와 함께 Base64로 인코딩된 사용자 이름과 비밀번호를 포함합니다. 예를 들어, "username"과 "password"를 인증 정보로 사용한다면 "username:password"를 Base64로 인코딩한 값인 "dXNlcm5hbWU6cGFzc3dvcmQ="을 Authorization 헤더에 포함시킵니다.

## Form Login 인증

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Form 기반 로그인을 이용하는 과정</p></figcaption></figure>
