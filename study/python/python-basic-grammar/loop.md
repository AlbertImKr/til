---
description: 반복문
---

# Loop

## for 문

```python
for 변수명 in 순회_가능한_객체 :
    # 코드
```

### List comprehension

```python
[표현식 for 항목 in 반복가능객체 if 조건] 

# 중첩 가능
[표현식 for 항목 in 반복가능객체 if 조건 for 항목 in 반복가능객체 if 조건] 
```

### 딕셔너리 및 집합 컴프리헨션

```python
# 딕셔너리 컴프리헨션
{표현식 for 항목 in 반복가능객체 if 조건}  # example {x: x**2 for x in range(5)}

# 집합 컴프리헨션
{표현식 for 항목 in 반복가능객체 if 조건}  # example {x**2 for x in range(5)}
```

## While 문

```python
while 조건:
    # 코드
```

## Break 문

**break** 문은 반복문(while,for)를 실행 중에 중단하고 탈출하는 명령어

## Continue, pass

Continue는 다음 반복 , pass 는 자니간다.

## 반복문 else

정상적으로 모든 반복이 종료시 실행 ( break나 예외 발생으로 인한 중단이 발생하지 않음)

## 참조

> [견고한 파이썬](https://www.books.weniv.co.kr/python)
>
> ChatGPT
>
> [https://www.geeksforgeeks.org/python-functions/](https://www.geeksforgeeks.org/python-functions/)
