---
description: 연산과 구문
---

# Operations and syntax

## 연사자란?

프로그래밍에서 데이터를 처리하고 분석하는 데 쓰이는 **기호**나 **키워드**

## 종류

### 산술 연산자

* `+` : 덧셈
* `-` : 뺄셈
* `*` : 곱셈
* `/` : 나눗셈 (값: `float` 타입)
* `//` : 나눗셈 (값: `int` 타입)
* `%` : 나머지 연산
* `**` : 제곱 연산

{% hint style="danger" %}
전/후위 연산자 지원하지 않음
{% endhint %}

### 비교 연산자

* `==` : 같음
* `!=` : 같지 않음
* `<` : 작음
* `<=` : 작거나 같음
* `>` : 큼
* `>=` : 크거나 같음&#x20;

{% hint style="warning" %}
컴퓨터는 소수를 이진수로 변환하여 저장하는데, 이 과정에서 정확한 값이 아닌 근사치로 저장됩니다. 따라서, 부동소숫점 수를 직접 비교하면 예상치 못한 결과가 나올 수 있습니다.



해결 방법:

* `math.isclose()` 함수 사용
{% endhint %}

### 논리 연산자

* **and**
* **or**
* **not**

### 할당 연산자

<table><thead><tr><th width="128">연산자</th><th>설명</th><th>예시</th><th>동등한 연산</th></tr></thead><tbody><tr><td><code>=</code></td><td>단순 할당</td><td><code>x = 5</code></td><td><code>x = 5</code></td></tr><tr><td><code>+=</code></td><td>덧셈 후 할당</td><td><code>x += 3</code></td><td><code>x = x + 3</code></td></tr><tr><td><code>-=</code></td><td>뺄셈 후 할당</td><td><code>x -= 2</code></td><td><code>x = x - 2</code></td></tr><tr><td><code>*=</code></td><td>곱셈 후 할당</td><td><code>x *= 4</code></td><td><code>x = x * 4</code></td></tr><tr><td><code>/=</code></td><td>나눗셈 후 할당 (실수 나눗셈)</td><td><code>x /= 2</code></td><td><code>x = x / 2</code></td></tr><tr><td><code>//=</code></td><td>나눗셈 후 할당 (정수 나눗셈)</td><td><code>x //= 2</code></td><td><code>x = x // 2</code></td></tr><tr><td><code>%=</code></td><td>나머지 연산 후 할당</td><td><code>x %= 3</code></td><td><code>x = x % 3</code></td></tr><tr><td><code>**=</code></td><td>거듭제곱 후 할당</td><td><code>x **= 2</code></td><td><code>x = x ** 2</code></td></tr></tbody></table>

### 식별 연산자

식별 연산자는 두 변수의 동일한 객체를 참조하고 있는지 확인하는데 사용

<table><thead><tr><th width="118">연산자</th><th width="271">설명</th><th width="137">예시</th><th>결과</th></tr></thead><tbody><tr><td><code>is</code></td><td>두 객체가 동일한 객체인지 비교</td><td><code>a is b</code></td><td><code>True</code> 또는 <code>False</code></td></tr><tr><td><code>is not</code></td><td>두 객체가 동일한 객체가 아닌지 비교</td><td><code>a is not c</code></td><td><code>True</code> 또는 <code>False</code></td></tr></tbody></table>

#### `is` vs `==`

* `is` 연산자가 `id` 를 기준으로 `주소`가 같은지 확인
* `==` 비교 연산자는 `값`이 같은지 확인

### 멤버 연산자

<table><thead><tr><th width="125">연산자</th><th width="271">설명</th><th width="238">예시</th><th>결과</th></tr></thead><tbody><tr><td><code>in</code></td><td>값이 시퀀스 또는 컬렉션에 존재하는지 확인</td><td><code>3 in [1, 2, 3]</code></td><td><code>True</code></td></tr><tr><td><code>not in</code></td><td>값이 시퀀스 또는 컬렉션에 존재하지 않는지 확인</td><td><code>4 not in [1, 2, 3]</code></td><td><code>True</code></td></tr></tbody></table>

## 참조

> [견고한 파이썬](https://www.books.weniv.co.kr/python)
>
> ChatGPT
