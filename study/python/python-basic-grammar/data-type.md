---
description: 자료형
---

# Data type

## 정수형(int)

정수는 0과 양의 정수, 음의 정수

### 진수

* 2진수 (Binary)는 `Ob`를 붙여서 표현
* 8진수 (Octal)는 `Oo`를 붙여서 표현
* 16진수 (Hexadecimal)는 `Ox`를 붙여서 표현
* 진수 변환 함수: bin(), oct(), hex()

{% hint style="warning" %}
Python에서 정수의 주소는 꼭 같지 않을 수 있다. 이는 Python의 메모리 관리와 객체 재사용 정책에 따라 달라집니다. 특히 작은 정수는 캐시되어 동일한 객체를 가리키는 반면, 큰 정수나 특정 조건에서 생성된 정수는 새로운 객체로 생성될 수 있습니다.



작은 정수는 일반적으로 -5에서 256 사이의 정수로, Python 인터프리터가 미리 생성하고 재사용합니다. 따라서 이 범위 내의 정수는 동일한 객체를 가리키므로 주소가 동일합니다. 그러나 이 범위를 벗어나는 정수는 새로운 객체로 생성될 수 있습니다.
{% endhint %}

## 실수형(float)

실수는 실제로 존재하눈 수

### float의 특수값

* **inf**: 양의 무한대를 나타내는 상수
* **-inf**: 음의 무한대를 나타내는 상수

```python
float("inf")
float("-inf")
```

### 문제점

* 연산이 항상 정확하진 않다.

### 해결방법

* `decimal` 사용

```python
import decimal

decimal.Decimal('.1') + decimal.Decimal('.2')
```

## 복소수형(complex)

실수와 허수의 합

## 문자열형 (String Type)

문자**열**

### **따옴표(`'`)** 또는 **큰 따옴표(`"`)**로 문자들을 감싼다

```python
example1 = 'hello'
example2 = "hi"
```

### **따옴표 3개(`'''`)**, **큰 따옴표 3개(`"""`)** 로 여러 문장을 감싼다

```python
multi_line_string1 = '''
This is a string that spans
multiple lines. You can write
as many lines as you want.
'''

multi_line_string2 = """
This is another string that spans
multiple lines. This method is
equally valid and useful.
"""
```

### 자주 사용하는 메서드

* lower(), upper()
* find() vs index()
  * 찾을 수 없는 문자열 일 경우&#x20;
    * find는 -1
      * index는 `ValueError: substring not found`
* count()
* strip()
* replace()
* split() / join()
* format()
* isalnum( ) / isdigit( ) / isalpha( ) / isascii( )
* rjust( ) / ljust( ) / center( )
* zfill()
* translate()
  * maketrans() 세 가지 사용법

<pre class="language-python"><code class="lang-python"># 1. 두 개의 동일한 길이의 문자열 인수 사용
<strong># 첫 번째 문자열의 각 문자를 두 번째 문자열의 대응하는 문자로 매핑합니다.
</strong>table = str.maketrans("abc", "123")
result = "abcdef".translate(table)
print(result)  # 출력: 123def

# 2.세 개의 인수 사용: 첫 번째와 두 번째 문자열은 매핑, 세 번째 문자열은 제거할 문자
# 첫 번째 문자열의 문자를 두 번째 문자열의 대응하는 문자로 매핑하고, 세 번째 문자열의 문자는 제거합니다.
table = str.maketrans("abc", "123", "def")
result = "abcdef".translate(table)
print(result)  # 출력: 123

# 3.딕셔너리를 사용한 매핑
# 문자(유니코드 코드 포인트)를 다른 문자 또는 None으로 매핑하는 딕셔너리를 전달할 수 있습니다.
# None으로 매핑된 문자는 제거됩니다.

table = str.maketrans({"a": "1", "b": "2", "c": "3", "d": None})
result = "abcde".translate(table)
print(result)  # 출력: 123e

</code></pre>

### 문자열 포메팅

1. `%` 연산자 사용
   * `%s`: 문자열
   * `%d`: 정수
   * `%f`: 부동 소수점 숫자
2. format() 메서드 사용&#x20;
3. f-string 사용

<pre class="language-python"><code class="lang-python"><strong># % 연산자 사용
</strong><strong>name = 'albert'
</strong><strong>age = 30
</strong><strong>'My name is %s, my age is %d' % (name,age)
</strong><strong>
</strong><strong># format() 메서드 사용
</strong>format_str = 'My name is {}'.format('albert')

# f-string 사용
name = 'Albert'
f-string = f'Myname is {name}'
</code></pre>

### 이스케이프 문자들

* `\n`: 줄바꿈
* `\t`: 탭
* `\r`: 커서를 현재 줄의 첨으로 이동
* `\"`,`\'`,`\\` 큰 따옴표, 작은 따옴표, 백슬래시

{% hint style="info" %}
`\r` 를 사용하여 출력을 덮어쓰고 진행 상태 표시줄 구현할 수 있어요.

```python
# ChatGPT에서 가져옴 
import time
import sys

def progress_bar(iteration, total, length=50):
    percent = ("{0:.1f}").format(100 * (iteration / float(total)))
    filled_length = int(length * iteration // total)
    bar = '█' * filled_length + '-' * (length - filled_length)
    sys.stdout.write(f'\r|{bar}| {percent}%')
    sys.stdout.flush()
    if iteration == total:
        print()

# 사용 예시
total_iterations = 100
for i in range(total_iterations + 1):
    progress_bar(i, total_iterations)
    time.sleep(0.1)
```
{% endhint %}

## 리스트(list)

## 튜플(tuple)

## 레인지(range)

## 집합(set)

## 프로즌셋(fronzenset)

## 딕셔너리(dict)

## 논리(bool)

* `True`: 참
* `False`: 거짓

{% hint style="info" %}
불리언 값은 숫자 1과 0으로 취급될 수 있습니다.

```python
print(True == 1)   # 출력: True
print(False == 0)  # 출력: True
print(True + 1)    # 출력: 2 (True는 1로 취급되므로 1 + 1 = 2)
print(False + 1)   # 출력: 1 (False는 0으로 취급되므로 0 + 1 = 1)
```
{% endhint %}

## None형

* 아무것도 없다

## 메서드 체이닝 가능

```python
target = '   Hello, World!  '
result = target.strip().lower().replace('hello','hi') # hi, world!
```

## 형 변환 가능

<table><thead><tr><th>Built-in Fucntion</th><th>기능</th><th data-hidden></th></tr></thead><tbody><tr><td>int()</td><td>정수로 변환</td><td></td></tr><tr><td>str()</td><td>문자열로 변환</td><td></td></tr><tr><td>float()</td><td>실수로 변환</td><td></td></tr><tr><td>list()</td><td>리스트로 변환</td><td></td></tr><tr><td>tuple()</td><td>튜플로 변환</td><td></td></tr><tr><td>dict()</td><td>딕셔너리로 변환</td><td></td></tr><tr><td>set()</td><td>셋으로 변환</td><td></td></tr></tbody></table>

{% hint style="danger" %}
닷(.)이 포함되여 있으면 정수로 변환되지 않는다.

```python
x = '1.0'
int(x) # ValueError: invalid literal for int() with base 10: '1.0'
```
{% endhint %}

## 참조

> [견고한 파이썬](https://www.books.weniv.co.kr/python)
>
> ChatGPT
