---
description: 변수
---

# Variable

변수는 어떤 값이 있는 위치를 가리킨다.

```mermaid
graph LR
    변수 --> id1["값의 주소"]
```

```python
age = 30
name = 'albert'fff
```

{% hint style="info" %}
python에서 변수는 값을 직접 저장하는 것이 아니라 객체에 대한 참조(주소)를 저장합니다. 변수 간의 할당은 실제 값을 복사하는 것이 아니라 객체에 대한 참조를 복사하는 것입니다.&#x20;
{% endhint %}

<pre class="language-python"><code class="lang-python">x = 10
y = x
z = y

x = 20

"""
<strong>결과:
</strong>x = 20
y = 10
z = 10
"""
</code></pre>

## 변수 이름

1. 알파펫(대소문자), 숫자, 인더스코어(\_)  ⭕️
2. 숫자로 시작 ❌
3. 파이썬의 키워드 (예: if, else, while 등)  ❌
4. 대소문자 구분 ⭕️

```python
# 올바른 변수 이름
student_name = "Alice"
age = 20
_gpa = 3.5
course_101 = "Mathematics"
EmployeeID = 12345
total_sales_2024 = 500000
isEnrolled = True

# 잘못된 변수 이름
2nd_place = "Bob"  # 숫자로 시작 ❌
class = "Physics"  # 파이썬 키워드 사용 ❌
first-place = 1    # 하이픈(-) 사용 ❌
```



## 변수의 타입

`type()` 함수를 사용하여 타입 확인

```python
age = 10
print(type(age)) # 출력: <class 'int'>
```

`dir()` 함수를 사용하여 해당 타입이 가진 속성과 메서드를 확인

```python
# 문자열 타입의 속성과 메서드 확인
print(dir(str))

['capitalize',    # 첫 글자를 대문자로 변환
 'center',        # 문자열을 지정된 너비로 가운데 정렬
 'count',         # 부분 문자열의 등장 횟수를 반환
 'encode',        # 문자열을 지정된 인코딩으로 변환
 'find',          # 부분 문자열을 찾아서 인덱스를 반환 (없으면 -1)
 'format',        # 문자열 포맷팅
 'index',         # 부분 문자열을 찾아서 인덱스를 반환 (없으면 예외 발생)
 'isalnum',       # 모든 문자가 영숫자인지 여부를 확인
 'isalpha',       # 모든 문자가 알파벳인지 여부를 확인
 'isdigit',       # 모든 문자가 숫자인지 여부를 확인
 'islower',       # 모든 문자가 소문자인지 여부를 확인
 'isupper',       # 모든 문자가 대문자인지 여부를 확인
 'join',          # 문자열의 모든 항목을 연결하여 하나의 문자열로 반환
 'lower',         # 모든 문자를 소문자로 변환
 'lstrip',        # 문자열 왼쪽의 공백을 제거
 'replace',       # 부분 문자열을 다른 문자열로 대체
 'rfind',         # 부분 문자열을 오른쪽에서 찾아서 인덱스를 반환 (없으면 -1)
 'rsplit',        # 오른쪽에서부터 분할
 'rstrip',        # 문자열 오른쪽의 공백을 제거
 'split',         # 구분자를 기준으로 문자열을 분할하여 리스트로 반환
 'startswith',    # 문자열이 특정 접두사로 시작하는지 여부를 확인
 'strip',         # 문자열 양쪽의 공백을 제거
 'title',         # 모든 단어의 첫 글자를 대문자로 변환
 'upper',         # 모든 문자를 대문자로 변환
 ...]         
```

## 변수 삭제

`del` 키워드를 사용하여 변수를 삭제

```python
del variable # NameError
```

## 참조

> [견고한 파이썬](https://www.books.weniv.co.kr/python)
>
> ChatGPT
