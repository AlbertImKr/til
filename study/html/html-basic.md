---
description: 기본
---

# HTML Basic

## Element

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## 기본 구조

```html
<!-- doctype은 모든 문서의 최상단에서 필수 요소로 사용되어야 한다. -->
<!-- html은 HTML Living Standard를 표현한다. -->
<!-- living Standard는 WHATWG에서 관리하는 웹표준이다. -->
<!-- HTML5 이지만 또 아니다. HTML5에서 W3C에서 관리했던 버전이다.  -->
<!DOCTYPE html>
<!-- html 태그는 HTMl 문서의 루트, 최상단 요소이며 주요하게 lang 속성을 통해 해당 페이지의 주 언어가 무엇인지 알려준다 -->
<html lang="en">
  <!-- head 태그는 기계가 읽을 수 있는 정보가 담고 있는 요소이다. -->
  <head>
    <!-- meta 태그는 다른 메타 관련 요소가 표현할 수 없는 다양한 정보를 담고 있는 요소이다. -->
    <!-- charset은 해당 문서의 문자 인코딩 방식을 나타낸다. -->
    <meta charset="UTF-8" />
    <!-- viewport는 뷰포트의 크기를 조절하는 속성이다. -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- title은 브라우저의 제목 표시줄이나 페이지의 탭에 표시되는 문서 제목을 정의한다. -->
    <!-- 검색엔진과 관련 있다 -->
    <title>Document</title>
  </head>
  <!-- body 태그는 HTML 문서의 내용을 나타내는 요소이다. -->
  <body></body>
</html>
```

## Block vs Inline

HTML에서 블록, 인라인, 인라인 블록 요소의 차이점을 정리한 표를 작성해드리겠습니다.

<table><thead><tr><th width="137">특성</th><th width="199">Block Elements</th><th width="225">Inline Elements</th><th>Inline-Block Elements</th></tr></thead><tbody><tr><td><strong>전체 가로폭</strong></td><td>✅</td><td>❌</td><td>❌</td></tr><tr><td><strong>크기 지정</strong></td><td>✅</td><td>❌</td><td>✅</td></tr><tr><td><strong>여백과 패딩</strong></td><td>all</td><td>좌, 우</td><td>all</td></tr><tr><td><strong>중첩</strong></td><td><p>다른 블록 요소 ✅</p><p>인라인 요소 ✅</p></td><td><p>주로 텍스트 ✅</p><p>다른 인라인 요소 ✅</p></td><td><p>인라인 블록 요소 ✅</p><p>인라인 요소 ✅</p></td></tr><tr><td><strong>예시 요소</strong></td><td><code>&#x3C;div></code>, <code>&#x3C;h1></code>, <code>&#x3C;p></code>, <code>&#x3C;section></code>, <code>&#x3C;header></code>, <code>&#x3C;footer></code></td><td><code>&#x3C;span></code>, <code>&#x3C;a></code>, <code>&#x3C;img></code>, <code>&#x3C;strong></code>, <code>&#x3C;em></code></td><td><code>&#x3C;img></code>, <code>&#x3C;button></code>, <code>&#x3C;input></code>, <code>&#x3C;label></code>, <code>&#x3C;iframe></code></td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## 마무리

기본 구조와 브록 및 인라인을 정리하면서 그 개념을 다시 확인하게 되고 그 의미에 대해 더 깊게 이해됐어요.

## 참조

> [https://en.wikipedia.org/wiki/HTML5](https://en.wikipedia.org/wiki/HTML5)
>
> ChatGPT
>
> [오름캠프 백엔드](https://www.books.weniv.co.kr/basecamp-html-css/chapter01/01-3)
>
> [https://developer.mozilla.org/](https://developer.mozilla.org/)

