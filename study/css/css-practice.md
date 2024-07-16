---
description: CSS 연습
---

# CSS Practice

## 결과

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

## 코드

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://unpkg.com/sanitize.css" rel="stylesheet">
    <title>자기소개서</title>
    <style>
        body {
            width: 800px;
        }

        #user-info {
            text-decoration: none;
            padding-left: 51px;
            white-space: nowrap;
            display: grid;
            grid-template-columns: 150px auto;
        }

        #user-info h2 {
            position: absolute;
            clip: rect(0 0 0 0);
            width: 1px;
            height: 1px;
            margin: -1px;
            overflow: hidden;
        }

        #user-info ul {
            list-style: none;
            padding: 0;
            margin-left: 40px;
        }

        #user-info img {
            margin-top: 12px;
            width: 150px;
            height: 150px;
            border-radius: 50%;
            display: inline;
        }

        #user-info ul li {
            position: relative;
            margin: 12px;
        }

        #user-info ul li span {
            font-size: 14px;
            color: #121314;
            position: absolute;
            left: 120px;
            bottom: 0;
        }

        #user-info ul li:first-child {
            margin-top: 0px;
            font-size: 24px;
            border-bottom: 1px solid #2E6FF2;
        }

        #user-info ul li:first-child span {
            color: #3075FF;
            margin-bottom: 7px;
        }

        #user-info ul li:last-child {
            background-color: #F3F5FA;
            border-radius: 8px;
            height: 44px;
            padding: 12px;
        }


        #user-info ul li:last-child span {
            font-size: 14px;
            bottom: 25%;
            padding-left: 12px;
        }

        #user-info ul li:last-child img {
            height: 20px;
            width: 20px;
            margin-right: 12px;
            margin-top: 0;
        }

        #introduction {
            padding-left: 51px;
            margin: 12px;
        }

        #introduction h2 {
            color: #2E6FF2;
            border-bottom: 1px solid #2E6FF2;
        }

        #skills {
            display: grid;
            grid-template-columns: 60px auto;
            padding-left: 51px;
            color: #2E6FF2;
            height: 40px;
            margin-bottom: 40px;
            margin: 12px;
        }

        #skills ul {
            list-style: none;
            padding: 0;
            margin-left: 40px;
            line-height: 40px;
        }

        #skills ul li {
            box-sizing: border-box;
            height: 40px;
            width: 85px;
            display: inline-block;
            background: #F3F5FA;
            border: 1px solid #D9DBE0;
            border-radius: 40px;
            color: #2E6FF2;
            text-align: center;
        }

        #project {
            padding-left: 51px;
            color: #2E6FF2;
            margin: 12px;
        }

        #project h2 {
            border-bottom: 1px solid #2E6FF2;
        }

        #project p {
            color: #121314;
        }

        #project img {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            display: inline;
        }

        #links {
            padding-left: 51px;
        }

        #links h2 {
            position: absolute;
            clip: rect(0 0 0 0);
            width: 1px;
            height: 1px;
            margin: -1px;
            overflow: hidden;
        }

        #links ul {
            list-style: none;
            padding: 0;
            margin: 12px;
        }

        #links ul li {
            position: relative;
            margin-bottom: 12px;
            background-color: #F3F5FA;
            border-radius: 8px;
            height: 44px;
            padding-left: 12px;
            line-height: 44px;
        }

        #links ul li img {
            height: 20px;
            width: 20px;
            margin-right: 12px;
        }

        #links ul li span {
            margin: 12px;
            padding-left: 12px;
        }
    </style>
</head>

<body>
    <article id="user-info">
        <h2>user-info</h2>
        <img src="image.png" alt="사진">
        <ul>
            <li> 스파이더맨 <span>spider-man</span> </li>
            <li>전화번호<span>010-1111-1222</span></li>
            <li>이메일<span>weniv@weniv.co.kr</span></li>
            <li>경력 사항<span>신입</span></li>
            <li><img src="link.png" alt="">기술
                블로그<Span>https://www.books.weniv.co.kr/</Span></li>
        </ul>
    </article>
    <article id="introduction">
        <h2>Introduction</h2>
        <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Fuga exercitationem libero excepturi consequuntur
            qui a, eligendi voluptates repellendus praesentium consectetur corrupti atque. Ex repellat repellendus dicta
            quibusdam quidem, vel earum?
        </p>
        <p>
            Lorem ipsum dolor sit amet consectetur, adipisicing elit. Temporibus ab deleniti distinctio vero totam autem
            eum ducimus dicta possimus perspiciatis delectus, sequi dolorum excepturi ad repudiandae omnis voluptatem,
            eveniet reiciendis.
        </p>
    </article>
    <article id="skills">
        <h2>Skills</h2>
        <ul>
            <li>Python</li>
            <li>Django</li>
        </ul>
    </article>
    <article id="project">
        <h2>Project</h2>
        <h3>프로젝트 예시</h3>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Corrupti nemo culpa provident non impedit dolor
            accusamus ex odit cumque officia quas animi commodi omnis voluptas, reiciendis minima vitae dignissimos
            dicta?</p>
    </article>
    <article id="links">
        <h2>links</h2>
        <ul>
            <li><img src="link.png" alt="">깃허브
                링크<Span>https://www.books.weniv.co.kr/</Span></li>
            <li><img src="link.png" alt="">프로젝트
                링크<Span>weniv.co.kr</Span></li>
            <li><img src="link.png" alt="">프로젝트 SNS
                링크<Span>https://github.com/weniv</Span></li>
        </ul>
    </article>
</body>

</html>
```
