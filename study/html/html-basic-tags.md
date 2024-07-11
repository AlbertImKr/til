---
description: 자주 사용하는 태크
---

# HTML Basic Tags

## \<div>: The Content Division element

* **스타일링**이나 **스크립팅**을 위한 콘테츠 분할 **블록** 요소

{% hint style="info" %}
div 보다 header, footer, main, section, article, nav 등 다양한 시매틱한 테그들을 사용하는 것을 권장

(검색엔진최적화, 코드 가독성, 접근성 등의 이유)
{% endhint %}

```markup
<div>
    오늘 날씨는 맑음
</div>
<div>
    내일 날씨도 맑음
</div>
```

## \<span>: The Content Span element

* **인라인** 컨테이너

```html
<p>오늘 <span style="color: skyblue;">날씨</span>는 맑음! </p>
```

## \<section>: The Generic Section element

* 문서의 **논리적** 구획을 나누어 주제별로 관련된 콘텐츠를 그룹하는 블록 요소
* 제목 요소를 자식으로 포함해야 함

```html
<section>
    <h2>날씨</h2>
    <p>오늘 날씨는 맑음</p>
</section>
```

## \<header>: The Header element

* 소개 또는 탐색에 도움을 주는 블록 콘테츠

```html
<article>
    <header>
        <h1>날씨</h1>
    </header>
    <p>오늘 날씨는 맑음!!</p>
</article>
```

## \<nav>: The Navigation Section element

* **navigation**으로서 현재 페이지 내, 또는 다른 페이지로의 링크정보를 담는 Section 요소

```html
<nav>
    <ul>
        <li><a href="#">오늘 날씨</a></li>
        <li><a href="#">내일 날씨</a></li>
        <li><a href="#">일주일 날씨</a></li>
    </ul>
</nav>
```

## \<footer>: The Footer element

* 상위 섹션 콘텐츠 또는 섹션 루트 요소의 **footer**
* 일반적으로 해당 색션의 작성자, 저작권 데이터 또는 관련 문서에 대한 링크정보가 포함될 수 있다.

```html
<body>
    <section>
        오늘 날씨는 맑음!
    </section>
    <footer>
        Written By Albert
    </footer>
</body>
```

## \<main>: The Main element

* \<body>의 **주요 내용**을 표현한다.
* 웹페이지에서 **한 번**만 사용할 수 있습니다.

```html
<body>
    <main>날씨</main>
    <section>
        오늘 날씨는 맑음!
    </section>
</body>
```

## \<article>: The Article Contents element

* **독립적**으로 배포되건 **재사용**될 수 있도록 의도된 구획

```html
<article>
    <h1>날씨</h1>
    <article>
        <h2>오늘</h2>
        <p>맑음</p>
    </article>
    <article>
        <h2>내일</h2>
        <p>흐림</p>
    </article>
</article>
```

## \<aside>: The Aside element

* 문서의 주요 내용과 **간접적**으로 연관된 부분 구획
* 사이드바, 각주, 광고 배너 등

```html
<aside>
    <p>비오는 날에는 파전!!</p>
</aside>
```

## \<h1>,\<h2>,\<h3>,\<h4>,\<h5>,\<h6>: The HTML Section Heading elements

* Heading 제목이다.

```html
<h1>h1</h1>
<h2>h2</h2>
<h3>h3</h3>
<h4>h4</h4>
<h5>h5</h5>
<h6>h6</h6>
```

<div align="center">

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="43"><figcaption></figcaption></figure>

</div>

## \<a>: The Anchor element

* 윕 페이지,파일, 이메일 주소, 동일한 페이지의 위치 또는 URL이 **주소**를 지정할 수 있는 모든 항목에 대한 **hyperlink**를 생성한다.
  * `href`: hypertext reference

```html
<a href="https://www.books.weniv.co.kr/basecamp-html-css">HTML/CSS 베이스캠프</a>
```

## \<p>: The Paragraph element

* **단락**을 나타납니다. (Paragraph)

```html
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Fuga corrupti alias possimus optio esse. Eos in asperiores ullam repellat minima cumque aspernatur dolores animi fugit non. Voluptatem ab dolorum iusto.</p>
```

## \<strong>: The Strong Importance element

* 콘텐츠의 **중요성**, **심각성** 또는 **긴급성**을 나타냅니다.
* 브라우저는 일반적으로 **굵은 글씨**로 렌더링한다.

{% hint style="info" %}
**\<b> vs. \<strong>**

\<strong> 요소는 중요한 콘텐츠이고 \<b>은 중요하는 것이 잘 보이게 하기 위해서이다.
{% endhint %}

```html
<p><strong>오늘</strong> 날씨는 맑음</p>
```

## \<br>: The Line Break element

* break(line break)

```html
<p>Genius is one percent inspiration,<br>ninety nine percent per</p>
```

{% hint style="warning" %}
paragraphs 사이에 여백을 만들기 위해 \<br>을 사용하지 말아야 한다.
{% endhint %}

## \<hr>: The Thematic Break (Horizontal Rule) element

* Thematic Break (주제 변경)

```html
<p>프로젝트 1: ...</p>
<hr />
<p>프로젝트 2: ...</p>
```

## \<code>: The Inline Code element

* 잛은 코드 조각(한 줄)을 나타난다

```html
<p><code>&lt;code&gt</code>는 코드의 조각을 표현한다.</p>
```

## \<pre>: The Preformatted Text element

* HTML에 작성한 내용 그대로 표현한다

```html
<pre>
          -
         ---
        -----
       -------
        -----
         ---
          -
</pre>
```

## \<ol>: The Ordered List element

* 번호가 매겨진 목록&#x20;
* `type`:`1`,`a`,`A`,`i`,`I`
* `start`: 시작 위치
* `reversed`: 역순

```html
<h2>공부 순서</h2>
<ol>
    <li>html</li>
    <li>css</li>
    <li>javascript</li>
</ol>
```

## \<ul>: The Unordered List element

* 순가가 없는 목록
* `type`: `circle`,`disc`,`square`

```html
<h2>기술 스택</h2>
<ul>
    <li>html</li>
    <li>css</li>
    <li>javascript</li>
</ul>
```

## \<img>: The Image Embed element

* 문서에 이미지를 삽입

```html
<img src="이미지 주소" alt="이미지 설명"> # src는 required
```

## 마무리

다양한 태그가 존재하는데 그 의미를 고민하지 않고 `div` ,`br` 이 편해서 막 사용했는데 그 태그의 의미에 맞게 고민해서 사용해야 겠어요.

## 참조

> [https://en.wikipedia.org/wiki/HTML5](https://en.wikipedia.org/wiki/HTML5)
>
> ChatGPT
>
> [오름캠프 백엔드](https://www.books.weniv.co.kr/basecamp-html-css/chapter01/01-3)
>
> [https://developer.mozilla.org/](https://developer.mozilla.org/)
