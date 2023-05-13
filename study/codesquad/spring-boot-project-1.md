# Spring Boot Project 1주차 회고

## 1. Classes naming: 단수 또는 복수

* [일반적으로 Collection 빼고 단수를 사용한다.](https://softwareengineering.stackexchange.com/questions/103720/classes-naming-singular-or-plural)

## 2. [Builder Pattern](../cs/design-pattern/builder-pattern.md)

### - 생성자 매개변수가 많다면 빌더를 고려하라. (이펙티브 자바:아이템 2)

* 점층적 생성자 패턴(telescoping constructor pattern)도 쓸 수도 있지만,매개 변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다.
* 자바빈즈 패턴에서는 객체 하나를 만드려면 메서드를 여러 개 호출해야 하고, 객체가 완전히 생성되기 전까지는 일관성(consistency)이 무너진 상태에 놓이게 된다.즉 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며 스레드 안전성을 얻으려면 프로그래머가 추가 작업을 해줘야만 한다.

#### before

<pre class="language-java"><code class="lang-java"><strong>//DTO
</strong><strong>public UsersForm mappingUsersForm() {
</strong>    UsersForm usersForm = new UsersForm();
    usersForm.setId(getId());
    usersForm.setEmail(getEmail());
    usersForm.setNickname(getNickname());
    return usersForm;
}
//DTO
public ProfileForm mappingProfileForm() {
    ProfileForm profileForm = new ProfileForm();
    profileForm.setEmail(getEmail());
    profileForm.setNickname(getNickname());
    return profileForm;
}
//DTO
public ProfileSettingForm mappingProfileSettingFormWithPassword() {
    ProfileSettingForm profileSettingForm = new ProfileSettingForm();
    profileSettingForm.setEmail(getEmail());
    profileSettingForm.setNickname(getNickname());
    return profileSettingForm;
}

//Entity
public User createNewUser(JoinForm joinForm) {
    User user = new User();
    user.setNickname(joinForm.getNickname());
    user.setEmail(joinForm.getEmail());
    user.setPassword(joinForm.getPassword());
    usersRepository.save(user);
    return user;
}
</code></pre>

#### after

<pre class="language-java"><code class="lang-java">//DTO
<strong>public UserForm mappingUsersForm() {
</strong>   return new UserForm.Builder()
	.id(id)
	.email(email)
	.nickname(nickname)
	.build();
}
//DTO
public ProfileForm mappingProfileForm() {
    return new ProfileForm.Builder()
		.email(email)
		.nickname(nickname)
		.build();
}
//DTO
public ProfileSettingForm mappingProfileSettingFormWithPassword() {
    return new ProfileSettingForm.Builder()
		.email(email)
		.nickname(nickname)
		.password(password)
		.build();
}
//Entity
public User createNewUser(JoinForm joinForm) {
    User user = new User.Builder(UsersRepository.atomicKey.incrementAndGet())
		.nickname(joinForm.getNickname())
		.email(joinForm.getEmail())
		.password(joinForm.getPassword())
		.build();
    usersRepository.save(user);
    return user;
}
</code></pre>

## 3. 객체의 책임과 역할

#### before

* User객체가 책임을 너무 많이 가졌다.

<pre class="language-java"><code class="lang-java">Class User {
<strong>    public UserForm mappingUsersForm() {
</strong>	return new UserForm.Builder()
		.id(id)
		.email(email)
		.nickname(nickname)
		.build();
    }

    public ProfileSettingForm mappingProfileSettingFormWithPassword() {
	return new ProfileSettingForm.Builder()
		.email(email)
		.nickname(nickname)
		.password(password)
		.build();
    }
}
</code></pre>

#### after

* 책임을 더욱 적절한 객체로 옮겼다.

```java
Class UserForm {
    public static UserForm from(User user) {
	return new Builder()
		.nickname(user.getNickname())
		.email(user.getEmail())
		.id(user.getId())
		.build();
    }
}
Class ProfileSettingForm {
     public static ProfileSettingForm from(User user) {
	return new Builder()
		.password(user.getPassword())
		.email(user.getEmail())
		.nickname(user.getNickname())
		.build();
      }
}

```

## 4.Spring Annotation

### - [Bean Annoation](../spring/spring-annotation.md#bean-annotations)

### - [Web Annotation](../spring/spring-annotation.md#web-annotations)

### - [Core Annotation](spring-boot-project-1.md#core-annotation)

## 6. 네이밍

### - 계층 별 네이밍 통일

#### before

* view 이름 불일치
* layer 메서드 이름 불일치

```java
//Controller
@GetMapping("/users/join")
public String addUser(User user){
    userService.createNewUser(user);
    // userRepository.save(user)
    return "member"
}
```

#### after

```java
//Controller
@GetMapping("/users/join")
public String saveUser(User user){
    userService.saveUser(user);
    // userRepository.save(user)
    return "join"
}
```

## 7. URL

### - Http Request Method 자체도 의미를 가진다.

### - RESTful Web Service HTTP methods 더 이해하기 좋다.

#### before

| url                    | method | 기능                    |
| ---------------------- | ------ | --------------------- |
| /users/login           | get    | 로그인 페이지 요청            |
| /users/login           | post   | 로그인 요청                |
| /users/join            | get    | 회원 가입 페이지 요청          |
| /users/join            | post   | 회원 가입 요청              |
| /users/{userId}        | get    | 회원 프로필 페이지 요청         |
| /users/{userId}/update | get    | 회원 프로필 설정 페이지 요청      |
| /users/{userId}/update | put    | 회월 프로필 설정 요청          |
| /users                 | get    | 모든 회원 맴버 페이지 요청       |
| /post                  | get    | 게시글 작성 페이지 요청         |
| /post                  | post   | 게시글 추가 요청             |
| /                      | get    | 모든 게스글 표시되는 메인 페이지 요청 |
| /post/{postId}         | get    | 게시글의 페이지 요청           |

#### after

| url                          | method | 기능                    |
| ---------------------------- | ------ | --------------------- |
| /users/login                 | get    | 로그인 페이지 요청            |
| /users/login                 | post   | 로그인 요청                |
| /users/join                  | get    | 회원 가입 페이지 요청          |
| /users                       | post   | 회원 가입 요청              |
| /users/{userId}/profile      | get    | 회원 프로필 페이지 요청         |
| /users/{userId}/profile/edit | get    | 회원 프로필 설정 페이지 요청      |
| /users/{userId}/profile      | put    | 회월 프로필 설정 요청          |
| /users                       | get    | 모든 회원 맴버 페이지 요청       |
| /posts/new,/posts/form       | get    | 게시글 작성 페이지 요청         |
| /posts                       | post   | 게시글 추가 요청             |
| /                            | get    | 모든 게스글 표시되는 메인 페이지 요청 |
| /posts/{postId}              | get    | 게시글의 페이지 요청           |

### - Idempotent Methods 멱등성

* 요청 방법은 의도한 효과가 "멱등적"인 것으로 간주됩니다. 해당 메서드를 사용하는 여러 동일한 요청의 서버는 단일 요청에 대한 효과와 동일합니다. 요청 방법 중 이 사양에서 정의한 PUT, DELETE 및 안전한 요청 방법 멱등적이다.
* 멱등성 메서드 문서: GET, HEAD, PUT, DELETE, OPTIONS, TRACE (en-US)&#x20;
* 비 멱등성 메서드 문서: POST,PATCH, CONNECT

## 8. `@Nested`

### - `@Nested`annotation을 사용하여 중첩된 테스트 클래스를 만들 수 있습니다.

#### before

<div align="left">

<figure><img src="../../.gitbook/assets/스크린샷 2023-04-09 오후 4.20.07.png" alt=""><figcaption></figcaption></figure>

</div>

#### after

<div align="left">

<figure><img src="../../.gitbook/assets/스크린샷 2023-04-09 오후 4.20.47.png" alt=""><figcaption></figcaption></figure>

</div>

> [https://developer.mozilla.org/ko/docs/Glossary/Idempotent](https://developer.mozilla.org/ko/docs/Glossary/Idempotent)
>
> [https://spoqa.github.io/2012/02/27/rest-introduction.html](https://spoqa.github.io/2012/02/27/rest-introduction.html)
>
> [https://www.baeldung.com/spring-core-annotations](https://www.baeldung.com/spring-core-annotations)
