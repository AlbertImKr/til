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

## 리스트 (list)

**가변적(mutable)**이고 순서가 있는 시퀀스 자료형이고 객체들의 **참조**(즉, **메모리 주소**)를 저장하는 배열입니다.&#x20;

### 생성

대괄호(**`[]`**) 안에 쉼표(**`,`**)로 구분된 데이터들을 넣는다

```python
x = [1, 2, 3] 

# 다차원 리스트
y = [[1,2,3],[1,2,3]]
```

{% hint style="info" %}
시퀀스형 기본 인덱스가 왜 0부터 시작할까요 ?\
이진수 관점에서 메모리를 절약할수 있다.
{% endhint %}

### 메서드

| 메서드                             | 설명                                                                      | 예시                                       |
| ------------------------------- | ----------------------------------------------------------------------- | ---------------------------------------- |
| `append(x)`                     | 리스트의 끝에 항목 `x`를 추가합니다.                                                  | `lst.append(3)`                          |
| `extend(iterable)`              | iterable의 모든 항목을 리스트의 끝에 추가합니다.                                         | `lst.extend([4, 5])`                     |
| `insert(i, x)`                  | 지정한 위치 `i`에 항목 `x`를 삽입합니다.                                              | `lst.insert(1, 'a')`                     |
| `remove(x)`                     | 리스트에서 첫 번째로 일치하는 항목 `x`를 제거합니다.                                         | `lst.remove(2)`                          |
| `pop([i])`                      | 지정한 위치 `i`에 있는 항목을 제거하고 반환합니다. 인덱스를 지정하지 않으면 마지막 항목을 제거하고 반환합니다.        | `lst.pop()` or `lst.pop(1)`              |
| `clear()`                       | 리스트의 모든 항목을 제거합니다.                                                      | `lst.clear()`                            |
| `index(x, start, end)`          | 리스트에서 첫 번째로 일치하는 항목 `x`의 인덱스를 반환합니다. `start`와 `end`로 검색 범위를 지정할 수 있습니다. | `lst.index(3)`                           |
| `count(x)`                      | 리스트에서 항목 `x`의 개수를 셉니다.                                                  | `lst.count(3)`                           |
| `sort(key=None, reverse=False)` | 리스트의 항목을 정렬합니다. `key`는 정렬 기준 함수, `reverse`는 역순 정렬 여부입니다.                | `lst.sort()` or `lst.sort(reverse=True)` |
| `reverse()`                     | 리스트의 항목 순서를 뒤집습니다.                                                      | `lst.reverse()`                          |
| `copy()`                        | 리스트의 얕은 복사본을 반환합니다.                                                     | `new_lst = lst.copy()`                   |

## 튜플 (Tuple)

**불변(immutable)**하고 순서가 있는 시퀀스 자료형

### 생성

소괄호 `()` 를 사용하고 그 안에 쉼표 `,` 로 구분

```python
x = (1,2,3)
```

### 메서드

| 메서드                   | 설명                                                                     | 예시           |
| --------------------- | ---------------------------------------------------------------------- | ------------ |
| `count(x)`            | 튜플에서 항목 `x`의 개수를 셉니다.                                                  | `t.count(2)` |
| `index(x,start, end)` | 튜플에서 첫 번째로 일치하는 항목 `x`의 인덱스를 반환합니다. `start`와 `end`로 검색 범위를 지정할 수 있습니다. | `t.index(3)` |

## 딕셔너리 (Dictionary)

키-값 쌍의 집합을 저장하는 데이터 구조

딕셔너리 키는 **유일**해야 한다.

### 생성

중괄호(`{}`)를 사용하여 생성하고 키와 값 상이에 콜론(`:`)을 사용하여 구분하며 각 키-값 쌍은 쉼표(`,`)로 구분

```python
dictionary_example = { key1: value1, key2: value2}
```

### 메서드

| 메서드                        | 설명                                                             | 예시                                   |
| -------------------------- | -------------------------------------------------------------- | ------------------------------------ |
| `clear()`                  | 딕셔너리의 모든 항목을 제거합니다.                                            | `d.clear()`                          |
| `copy()`                   | 딕셔너리의 얕은 복사본을 반환합니다.                                           | `d_copy = d.copy()`                  |
| `fromkeys(seq, v)`         | `seq`의 요소를 키로 하고, `v`를 값으로 하는 새로운 딕셔너리를 생성합니다. 기본값은 `None`입니다. | `dict.fromkeys(['a', 'b'], 0)`       |
| `get(key, default)`        | 지정한 키에 대응하는 값을 반환합니다. 키가 없으면 기본값을 반환합니다. 기본값은 `None`입니다.       | `d.get('key', default_value)`        |
| `items()`                  | 딕셔너리의 모든 키-값 쌍을 포함하는 뷰 객체를 반환합니다.                              | `d.items()`                          |
| `keys()`                   | 딕셔너리의 모든 키를 포함하는 뷰 객체를 반환합니다.                                  | `d.keys()`                           |
| `pop(key, default)`        | 지정한 키에 대응하는 값을 반환하고, 해당 키를 딕셔너리에서 제거합니다. 키가 없으면 기본값을 반환합니다.    | `d.pop('key', default_value)`        |
| `popitem()`                | 딕셔너리에서 마지막으로 삽입된 키-값 쌍을 제거하고 반환합니다.                            | `d.popitem()`                        |
| `setdefault(key, default)` | 키가 딕셔너리에 없으면 키를 추가하고, 기본값을 반환합니다. 기본값은 `None`입니다.              | `d.setdefault('key', default_value)` |
| `update([other])`          | 다른 딕셔너리나 iterable의 키-값 쌍을 사용하여 딕셔너리를 갱신합니다.                    | `d.update({'key1': 1, 'key2': 2})`   |
| `values()`                 | 딕셔너리의 모든 값을 포함하는 뷰 객체를 반환합니다.                                  | `d.values()`                         |

## 셋 (Set)

순서가 없고 중복된 값도 허용하지 않는 자료형

### 생성

중괄호 `{}` 를 사용하고 그 안에 쉼표`,`로 구분

```python
set_example = set([1,2,3,3,3]) # set_example = {1,2,3}
```

### 메서드

| 메서드                                  | 설명                                                      | 예시                                 |
| ------------------------------------ | ------------------------------------------------------- | ---------------------------------- |
| `add(elem)`                          | 집합에 요소를 추가합니다. 이미 존재하면 아무런 동작을 하지 않습니다.                 | `s.add(3)`                         |
| `clear()`                            | 집합의 모든 요소를 제거합니다.                                       | `s.clear()`                        |
| `copy()`                             | 집합의 얕은 복사본을 반환합니다.                                      | `s_copy = s.copy()`                |
| `difference(*others)`                | 하나 이상의 다른 집합과의 차집합을 반환합니다.                              | `s.difference(t)`                  |
| `difference_update(*others)`         | 집합을 다른 집합들과의 차집합으로 업데이트합니다.                             | `s.difference_update(t)`           |
| `discard(elem)`                      | 집합에서 요소를 제거합니다. 요소가 없어도 에러가 발생하지 않습니다.                  | `s.discard(3)`                     |
| `intersection(*others)`              | 하나 이상의 다른 집합과의 교집합을 반환합니다.                              | `s.intersection(t)`                |
| `intersection_update(*others)`       | 집합을 다른 집합들과의 교집합으로 업데이트합니다.                             | `s.intersection_update(t)`         |
| `isdisjoint(other)`                  | 다른 집합과 교집합이 없는지 확인합니다. 교집합이 없으면 True, 있으면 False를 반환합니다. | `s.isdisjoint(t)`                  |
| `issubset(other)`                    | 집합이 다른 집합의 부분집합인지 확인합니다.                                | `s.issubset(t)`                    |
| `issuperset(other)`                  | 집합이 다른 집합의 상위집합인지 확인합니다.                                | `s.issuperset(t)`                  |
| `pop()`                              | 집합에서 임의의 요소를 제거하고 반환합니다. 집합이 비어 있으면 KeyError를 발생시킵니다.   | `s.pop()`                          |
| `remove(elem)`                       | 집합에서 요소를 제거합니다. 요소가 없으면 KeyError를 발생시킵니다.               | `s.remove(3)`                      |
| `symmetric_difference(other)`        | 다른 집합과의 대칭차집합을 반환합니다.                                   | `s.symmetric_difference(t)`        |
| `symmetric_difference_update(other)` | 집합을 다른 집합과의 대칭차집합으로 업데이트합니다.                            | `s.symmetric_difference_update(t)` |
| `union(*others)`                     | 하나 이상의 다른 집합과의 합집합을 반환합니다.                              | `s.union(t)`                       |
| `update(*others)`                    | 집합을 다른 집합들과의 합집합으로 업데이트합니다.                             | `s.update(t)`                      |

## 참조

> [견고한 파이썬](https://www.books.weniv.co.kr/python)
>
> ChatGPT
