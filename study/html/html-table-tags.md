---
description: 표
---

# HTML Table Tags

## \<table>: The Table element

* 테이블 생성 즉 2차원 테이블을 표시하는 사용한다.
* `<tr>`: table row
* `<th>`: table header
* `<td>`: table data
* `<caption>`: 제목이나 설명
* `<thead>`: 제목 열 그룹
* `<tbody>`: 데이터 그룹
* `<tfoot>`: 요약 그룹
* `<colspan>`: 열 병합
* `<rowspan>`: 행 병합
* `<colgroup>`: 열 그룹
* `<col>`: 열

```html
<table>
    <caption>덴마크 다이어트 식단표</caption>
    <thead>
        <tr>
            <th></th>
            <th>월요일</th>
            <th>화요일</th>
            <th>수요일</th>
            <th>목요일</th>
            <th>금요일</th>
            <th>토요일</th>
            <th>일요일</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>아침</th>
            <td colspan="7">자몽, 계란 1~2개, 블랙 커피</td>
        </tr>
        <tr>
            <th>점심</th>
            <td>계란 2개, 자몽</td>
            <td>계란 2개, 토마토, 커피</td>
            <td colspan="3">계란 2개, 시금치, 커피</td>
            <td>과일샐러드 마음껏</td>
            <td>닭고기, 토마토, 당근, 양배추, 자몽, 커피</td>
        </tr>
        <tr>
            <th>저녁</th>
            <td>계란 2개, 콤비네이션 샐러드, 건토스트 1조각, 자몽, 커피</td>
            <td>스테이크, 토마토, 오이, 양상추, 올리브, 커피</td>
            <td>양갈비 2개, 셀러리 ,오이, 토마토, 티</td>
            <td>계란 2개, 코티지 치즈, 마른 토스트 한 조각, 양배추</td>
            <td>생선 샐러드, 드라이 토스트, 자몽</td>
            <td>스테이크, 셀러리, 오이, 토마토, 커피 듬뿍</td>
            <td>냉동 닭고기, 토마토, 자몽</td>
        </tr>
    </tbody>
</table>
```

## 마무리

테이블이 한 행씩 나열해서 작성하기 불편하는 점이 있지만 테이블로 표현할 때 유용하게 사용할 수 있을 것 같아요.

## 참조

> [https://en.wikipedia.org/wiki/HTML5](https://en.wikipedia.org/wiki/HTML5)
>
> ChatGPT
>
> [오름캠프 백엔드](https://www.books.weniv.co.kr/basecamp-html-css/chapter01/01-3)
>
> [https://developer.mozilla.org/](https://developer.mozilla.org/)
