# OAuth 2 인증

## 아키텍처의 구성 요소

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption><p>OAuth 2 아키텍처의 주 구성 요소</p></figcaption></figure>

## 그랜트(grant) 유형

### Authorization Code(승인 코드)

* 가장 일반적으로 사용되는 Grant Type입니다.
* 웹 애플리케이션과 서버 간의 인증을 위해 사용됩니다.
* 인증 코드를 얻고, 이를 서버에 교환하여 액세스 토큰을 받습니다

<figure><img src="../../../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption><p>승인 코드 그랜트 유형</p></figcaption></figure>

### Implicit(암목적)

* 클라이언트 사이드 웹 애플리케이션에서 주로 사용됩니다.
* 인증 코드를 사용하지 않고, 액세스 토큰을 바로 반환합니다.
* 보안상의 이유로 권장되는 방식은 아니며, Authorization Code 방식의 대안으로 사용됩니다.

### Resource Owner Password Credentials(암호)

* 클라이언트가 사용자의 자격 증명을 직접 사용하여 인증하는 방식입니다.
* 보안 상의 이유로 권장되지 않습니다. 주로 신뢰할 수 있는 애플리케이션과의 통합 시에만 사용해야 합니다.

<figure><img src="../../../.gitbook/assets/image (8) (2).png" alt=""><figcaption><p>암호 그랜트 유형</p></figcaption></figure>

### Refresh Token(갱신 토큰)

* 액세스 토큰의 만료 시간을 연장하기 위해 사용됩니다.
* 유효한 리프레시 토큰을 사용하여 새로운 액세스 토큰을 얻습니다.

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption><p>Refresh Token(갱신 토큰) 그랜트 유형</p></figcaption></figure>

### Client Credentials(클라이언트 자격 증명)

* 클라이언트 애플리케이션이 자신의 자격 증명으로 인증하는 방식입니다.
* 사용자에 대한 액세스 권한 없이 클라이언트 자체의 자원에 접근할 때 사용됩니다.

<figure><img src="../../../.gitbook/assets/image (11) (2) (1).png" alt=""><figcaption><p>클라이언트 자격 증명 그랜트 유형</p></figcaption></figure>
