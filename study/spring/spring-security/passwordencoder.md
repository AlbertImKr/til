# PasswordEncoder

저부닷 NoOpPasswordEncoder

* 암호를 인코딩하지 않는다

## StandardPasswordEncoder

* SHA-256을 이용해 암호를 해시한다
* 이제 구식이다.
* SHA-256은 입력 데이터를 256비트(32바이트)의 고정 길이 해시 값으로 변환하는 단방향 해시 함수입니다
* 비밀번호 저장에는 솔트(salt)와 추가적인 보안 기능을 제공하는 알고리즘인 Pbkdf2PasswordEncoder, BCryptPasswordEncoder, SCryptPasswordEncoder와 같은 비밀번호 인코딩 알고리즘이 권장됩니다.
  * 솔트(Salt)는 비밀번호 해싱에서 사용되는 보안적인 요소로, 각각의 비밀번호에 대해 고유한 추가 데이터입니다. 솔트는 해시 함수에 입력되기 전에 비밀번호와 결합되어 해싱됩니다

### 특징

1. 일방향성: 해시 함수는 입력 값을 해시 값으로 변환하는 과정에서 단방향으로 동작합니다. 즉, 해시 값에서 입력 값을 역산하여 복구하는 것은 매우 어렵거나 불가능합니다.
2. 고정 길이 출력: SHA-256은 항상 256비트(32바이트)의 고정 길이 해시 값을 생성합니다. 입력 데이터의 크기에 상관없이 항상 동일한 길이의 출력을 생성합니다.
3. 고유성: SHA-256은 서로 다른 입력에 대해 거의 확률적으로 유일한 해시 값을 생성합니다. 즉, 두 개의 다른 입력이 있으면 해시 값도 매우 다른 값이 될 가능성이 높습니다.
4. 저항성: SHA-256은 충돌 저항성이 강화된 알고리즘입니다. 충돌은 두 개의 서로 다른 입력이 동일한 해시 값을 생성하는 상황을 의미합니다. SHA-256은 충돌을 매우 어렵게 만들어, 입력 값을 약간만 변경해도 완전히 다른 해시 값을 생성하도록 설계되었습니다.

*

## Pbkdf2PasswordEncoder

* PBKDF2는 Password-Based Key Derivation Function 2의 약자입니다.
* 이는 SHA-1이나 SHA-256과 같은 암호화 해시 함수를 여러 번 적용하여 안전한 비밀번호 해시를 생성하는 데 널리 사용되는 알고리즘입니다.
* PBKDF2는 계산 비용을 결정하는 반복 횟수를 구성할 수 있습니다. 반복 횟수를 늘릴수록 무차별 대입 공격에 대해 더 강력해지지만, 비밀번호 해싱에 필요한 처리 시간도 증가합니다.
* PBKDF2에는 내장된 솔트 생성 기능이 없습니다. 솔트는 각 비밀번호마다 별도로 생성하고 저장해야 보안성이 높아집니다.
* 신뢰성과 안전성이 높은 비밀번호 해싱 알고리즘으로 알려져 있습니다.
* PBKDF2는 Password-Based Key Derivation Function 2의 약자입니다.
* 이는 SHA-1이나 SHA-256과 같은 암호화 해시 함수를 여러 번 적용하여 안전한 비밀번호 해시를 생성하는 데 널리 사용되는 알고리즘입니다.
* PBKDF2는 계산 비용을 결정하는 반복 횟수를 구성할 수 있습니다. 반복 횟수를 늘릴수록 무차별 대입 공격에 대해 더 강력해지지만, 비밀번호 해싱에 필요한 처리 시간도 증가합니다.
* PBKDF2에는 내장된 솔트 생성 기능이 없습니다. 솔트는 각 비밀번호마다 별도로 생성하고 저장해야 보안성이 높아집니다.
* 신뢰성과 안전성이 높은 비밀번호 해싱 알고리즘으로 알려져 있습니다.

## BCryptPasswordEncoder

* BCrypt는 또 다른 널리 사용되는 비밀번호 해싱 알고리즘입니다.
* 내장된 솔트 생성 기능을 포함하며, 생성된 해시 내에서 솔트를 자동으로 처리합니다.
* BCrypt는 Blowfish 암호화 알고리즘을 사용합니다.

## SCryptPasswordEncoder

* SCrypt는 메모리 하드 함수로 알려진 알고리즘입니다.
* 메모리 하드 함수는 암호화 작업을 수행하는 동안 많은 메모리를 필요로 하므로, 공격자가 효율적인 하드웨어를 사용해 공격을 시도하는 것을 어렵게 만듭니다.
* SCrypt는 내장된 솔트 생성 기능을 포함하고, 솔트를 생성된 해시 내에 자동으로 저장합니다.
* PBKDF2와 비교하여 더 큰 메모리 요구사항과 더 높은 계산 비용이 필요하며, 보안성이 높은 비밀번호 해싱 알고리즘으로 알려져 있습니다.

## 동작 방식

<figure><img src="../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption><p>접두사로 판단</p></figcaption></figure>

## SSCM 필수 기능

### 스프링 시큐리터 암호화 모듈 SSCM (Single Sign-On(SSO) Client Manager)

* 키 생성기
  * 해성 및 암호화 알고리즘을 위한 키를 생성하는 객체

```java
BytesKeyGenerater keyGenerator = KeyGenerator.secureRandom(16);
```

* 암호기
  * 데이터를 암호화 및 복호화하는 객체

```java
String salt = KeyGenerator.string().generateKey();
String password = "secret";
String valueToEncrypt = "Hello";

BytesEncrptor e = Encryptors.standard(password, salt);
byte[] encrypted = e.encrypt(valueToEncrypt.getBytes());
byte[] decrypted = e.decrypt(encrypted);
```
