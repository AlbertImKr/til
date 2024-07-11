---
description: 양식
---

# HTML Form Tags

## \<form>: The Form element

* 데이터 제출을 위한 대화형 컨트롤이 포함된 문서 섹션이다.

### form 제출 속성

* `action`: 데이터 제출을 처리하는 URL
* `enctype`: 데이터 제출의 MIME 유형(`method`: `post`)
  * `application/x-www-form-urlencoed`: 기본 값
  * `multipart/form-data`: 파일(if `type`=`file`)
  * `text/plain`: 디버겅 목적으로 유용하다
* `method`:  HTTP 메서드
* `novalidate`: 양식의 유효성 검증을 건너뜀
* `target`: 양식 제출의 결과를 표시할 위치

```html
<form action="제출_URL" method="post" enctype="application/x-www-form-urlencoded">
    <label for="name">이름:</label>
    <input type="text" id="name" name="name">
    <label for="email">이메일:</label>
    <input type="email" id="email" name="email">
    <button type="submit" value="제출">
</form>
```

## \<button>: The Button element

* 활성화하는 대화형 요소다 활성화되면 데이터 제출, 대화 상장 열기 등의 작업을 수행할 수 있다.
* `type`:
  * `submit`: 데이터를 서버에 제출한다.(in `<form>`)
  * `reset`: 모든 컨트롤을 초기값으로 재설정
  * `button`: 기본 동작이 없다. 스크립트로 이벤트를 수집할 수 있다.
    * 데이터를 제출용이 아니라면 타입을 `button`으로 지정해야 합니다.

```html
<button type="submit" value="제출">
```

## \<input>: The Input (Form Input) element

* 사용자로부터 데이터를 받기위한 대화형 컨트롤
* [Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attributes) 는 많지만 간단하다. 필요할 때 확인

```html
<input id="user-name" name="user-name" type="text">
<input id="password" name="password" type="password">
```

## \<label>: The Label element

* 사용자 사용하는 항목에 대한 label이다.
* `<input>`또는 `<textarea>` 와 같은 form 컨트롤과 연결하여 사용한다.
* 연결 방식
  * `for`(in `<label>`)-`id`(in `<input>` 등) 방식
  * `<label>` 안으로 (`<!input>`)중첩

```html
<label for="user-name">유저 이름</label>
<input id="user-name" name="user-name" type="text">
```

## \<select>: The HTML Select element

* 옵션 메뉴를 제공하는 컨트롤

```html
<select name="dinner" id="dinner">
    <option value="none">메뉴를 선택해주세요</option>
    <optgroup label="중국집 메뉴">
        <option value="짜장면">짜장면</option>
        <option value="짬뽕">짬뽕</option>
        <option value="탕수육">탕수육</option>
    </optgroup>
    <optgroup label="한식집 메뉴">
        <option value="비빔밥">비빔밥</option>
        <option value="불고기">불고기</option>
        <option value="김치찌개">김치찌개</option>
    </optgroup>
</select>
```

## \<fieldset>: The Field Set element

* 웹 form 내에서 여러 컨트롤과 레이블(`<label>`)을 그룹화한다.

```html
<form action="#">
    <fieldset>
        <legend>성별</legend>
        <label for="male">남</label>
        <input type="radio" name="gender" id="male">
        <label for="female">녀</label>
        <input type="radio" name="gender" id="female">
    </fieldset>
    <button type="submit">제출</button>
</form>
```

## \<textarea>: The Textarea element

* 여러줄의 text를 입력받을 수 있다.
* 속성
  * `cols`:  너비
  * `rows`: 보여주는 입력 줄
  * `maxlength`: 최대 길이
  * `minlength`: 최소 길이
  * `placeholder`: 힌트

```html
<form action="#">
    <label for="comments">의견:</label><br>
    <textarea id="comments" name="comments" cols="50" rows="10" maxlength="500" 
        minlength="10" placeholder="여기에 의견을 입력하세요..."></textarea>
    <button type="submit">제출</button>
</form>
```

## 마무리

form은 요즘 많이 사용하지 않다고 하지만  아직까지 실제로 여전히 널리 사용되며 데이터를 전송하는 기본 방법 중 하나로서 그것을 알고 넘어간다고 생각해요.&#x20;

## 참조

> [https://en.wikipedia.org/wiki/HTML5](https://en.wikipedia.org/wiki/HTML5)
>
> ChatGPT
>
> [오름캠프 백엔드](https://www.books.weniv.co.kr/basecamp-html-css/chapter01/01-3)
>
> [https://developer.mozilla.org/](https://developer.mozilla.org/)
