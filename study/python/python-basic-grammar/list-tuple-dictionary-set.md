---
description: 리스트, 튜플, 딕셔너리, 셋
---

# List,Tuple,Dictionary,Set

## 시퀀스 자료형

여러 개의 항목들이 **순서대**로 나열된 구조를 나타내는 데이터 유형

### 종류

* 리스트(List)
* 문자열(String)
* 튜플(Tuple)
* 바이트(Bytes)
* 바이트배열(Bytearray)
* 범위(range)

### 특징

<table><thead><tr><th width="199">특징</th><th width="316">설명</th><th>예시 (리스트 기준)</th></tr></thead><tbody><tr><td><p>인덱싱</p><p>(Indexing)</p></td><td>시퀀스 내 특정 위치의 항목에 접근</td><td><code>my_list[0]</code></td></tr><tr><td><p>슬라이싱</p><p>(Slicing)</p></td><td>시퀀스의 부분집합을 얻음</td><td><code>my_list[1:3]</code></td></tr><tr><td>특정 항목의 포함 여부(Membership)</td><td>특정 항목이 시퀀스에 포함되어 있는지 확인</td><td><code>3 in my_list</code></td></tr><tr><td><p>연결 및 반복</p><p>(Iteration)</p></td><td>시퀀스의 항목들을 하나씩 처리</td><td><code>for item in my_list:</code></td></tr><tr><td><p>내장 함수</p><p>(Built-in Functions)</p></td><td>시퀀스와 함께 사용할 수 있는 다양한 함수</td><td><code>len(my_list)</code>, <code>min(my_list)</code>,<br><code>max(my_list)</code></td></tr></tbody></table>

### 패킹 vs 언패킹

#### 패킹

* 여러 값을 하나의 시퀀스 자료형에 묶는 작업

```python
my_tuple = 1, 2, 3
```

#### 언패킹

* 하나의 시퀀스 자료형을 여러 변수로 나누는 작업

```python
a, b, c = my_tuple
```

#### 별표(\*)를 이용한 언패킹

```python
a, *b, c = 1, 2, 3, 4, 5 # a = 1 , b = [2, 3, 4] , c = 5
```

## 참조

> [견고한 파이썬](https://www.books.weniv.co.kr/python)
>
> ChatGPT
