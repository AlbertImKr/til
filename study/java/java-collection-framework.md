# Java Collection Framework

## Collection

컨테이너라고도 하는 컬렉션은 단순히 여러 요소를 단일 단위로 그룹화하는 개체입니다. 컬렉션은 집계 데이터를 저장, 검색, 조작 및 전달하는 데 사용됩니다. 컬렉션은 집계 데이터를 저장, 검색, 조작 및 전달하는 데 사용됩니다. 일반적으로 포커 핸드(카드 모음), 메일 폴더(문자 모음) 또는 전화번호부(전화 번호에 대한 이름 매핑)와 같은 자연 그룹을 형성하는 데이터 항목을 나타냅니다.

## Data structure

일반적으로 데이터에 대한 효율적인 접근을 위해 선택되는 데이터 구성, 관리 및 저장 형식입니다. 보다 정확하게는 데이터 구조는 데이터 값들과 그들 사이의 관계 및 데이터에 적용될 수 있는 기능이나 연산의 집합이다.



## What Is a Collections Framework?

컬렉션 프레임워크는 컬렉션을 표현하고 조작하기 위한 통합 아키텍처입니다. 모든 컬렉션 프레임워크에는 다음이 포함됩니다.

* Interfaces: 컬렉션을 나타내는 추상 데이터 유형입니다. 인터페이스를 사용하면 컬렉션을 표현의 세부 사항과 독립적으로 조작할 수 있습니다. 객체 지향 언어에서 인터페이스는 일반적으로 계층 구조를 형성합니다.
* Implementations: 컬렉션 인터페이스의 구체적인 구현입니다. 본질적으로 이들은 재사용 가능한 데이터 구조입니다.
* Algorithms: 컬렉션 인터페이스를 구현하는 객체에서 검색 및 정렬과 같은 유용한 계산을 수행하는 메서드입니다. 알고리즘은 다형성(polymorphic)이라고 합니다. 즉, 적절한 컬렉션 인터페이스의 다양한 구현에서 동일한 메서드를 사용할 수 있습니다. 본질적으로 알고리즘은 재사용 가능한 기능입니다.

## 핵심 Collection Interfaces

### Collection

* Collection 계층 구조의 root입니다. Collection은 요소로 알려진 객체의 그룹니다. Collection 인터페이스는 모든 collections이 구현하는 최소 공통 요소이며  최대 일반성이 필요할 때  collections을 전달하고 조작하는 데 사용됩니다. 일부 유형의 컬렉션은 중복 요소를 허용하고 일부는 허용하지 않습니다. 일부는 정렬되여 있고 일부는 정렬되여 있지 않다. Java 플랫폼은 이 인터페이스의 직접적인 구현을 제공하지 않지만 Set 및 List와 같은 보다 구체적인 하위 인터페이스의 구현을 제공합니다.

### Set

* 중복 요소를 포함할 수 없는 컬렉션입니다. 이 인터페이스는 수학적 집합 추상화를 모델링하고 포커 핸드를 구성하는 카드, 학생의 일정을 구성하는 과정 또는 기계에서 실행되는 프로세스와 같은 집합을 나타내는 데 사용됩니다.

### List

* 정렬된 컬렉션(시퀀스라고도 함). Lists에는 중복 요소가 포함될 수 있습니다.  List의 사용자는 일반적으로 list에서 각 요소가 삽입되는 위치를 정확하게 제어할 수 있으며 정수 인덱스(위치)로 요소에 액세스할 수 있습니다.

### Queue

* 처리하기 전에 여러 요소를 보유하는 데 사용되는 컬렉션입니다. 기본 Collection operations 외에도 Queue는 추가 삽입, 추출 및 검사 operations을 제공합니다.
* Queues은 일반적으로 반드시 그런 것은 아니지만 FIFO(선입선출) 방식으로 요소를 정렬합니다. 예외 중에는 제공된 comparator 또는 요소의 natural 순서에 따라 요소를 정렬하는 priority queues이 있습니다. 사용된 순서가 무엇이든 queue의 헤드는 remove 또는 poll 호출에 의해 제거되는 요소입니다. FIFO queue에서 모든 새 요소는 queue의 끝에 삽입됩니다. 다른 종류의 queues은 다른 배치 규칙을 사용할 수 있습니다. 모든 Queue 구현은 순서 지정 속성을 지정해야 합니다.

### Deque

* 처리하기 전에 여러 요소를 보유하는 데 사용되는 컬렉션입니다. 기본 Collection operations 외에도 Deque는 추가 삽입, 추출 및 검사 operations을 제공합니다.
* Deques는 FIFO(선입선출) 및 LIFO(후입선출) 방식을 모두 사용할 수 있습니다. deque에서 모든 새 요소는 양쪽 긑에서 삽입, 검색 및 제거될 수 있습니다.

### Map

* 키를 값에 매핑하는 객체입니다. Map에는 중복 키가 포함될 수 없습니다. 각 키는 최대 하나의 값에 매핑할 수 있습니다.

### SortedSet

* 요소를 오름차순으로 유지하는 Set입니다. 정렬을 활용하기 위해 몇 가지 추가 작업이 제공됩니다.Sorted sets은 단어 목록 및 회원 목록과 같이 naturally 정렬된 집합에 사용됩니다.

### SortedMap

* 오름차순 키 순서로 매핑을 유지 관리하는 Map입니다. 이것은 SortedSet의 Map 아날로그입니다. 정렬된  Map은 사전 및 전화번호부와 같이 자연스럽게 정렬된 키/값 쌍 모음에 사용됩니다.&#x20;

## 구조

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Java Collection Framework</p></figcaption></figure>

## Benefits of the Java Collections Framework

Java 컬렉션 프레임워크는 다음과 같은 이점을 제공합니다.

* 프로그래밍 노력을 감소:  Collections Framework는 유용한 데이터 구조와 알고리즘을 제공함으로써 프로그램 작동에 필요한 낮은 수준의 "배관"이 아닌 프로그램의 중요한 부분에 집중할 수 있도록 해줍니다.
* 프로그램 속도 및 품질 향상: 이 컬렉션 프레임워크는 유용한 데이터 구조 및 알고리즘의 고성능, 고품질 구현을 제공합니다. 각 인터페이스의 다양한 구현은 상호 교환 가능하므로 컬렉션 구현을 전환하여 프로그램을 쉽게 조정할 수 있습니다. 자신만의 데이터 구조를 작성하는 번거로움에서 벗어나 프로그램의 품질과 성능을 개선하는 데 더 많은 시간을 할애할 수 있습니다.
* 관련 없는 APIs 간의 상호 운용성을 허용: Collection 인터페이스는 APIs가 Collection을 앞뒤로 전달하는 전문언어입니다. 내 네트워크 관리 API가 노드 이름 모음을 제공하고 GUI 도구 키트가 열 머리글 모음을 예상하는 경우 API는 독립적으로 작성되었더라도 원활하게 상호 운용됩니다.
* 새로운 APIs를 배우고 사용하기 위한 노력 감소: 많은 API는 자연스럽게 입력에 대한 컬렉션을 가져와 출력으로 제공합니다. 과거에는 이러한 각 API에 컬렉션 조작을 전담하는 작은 하위 API가 있었습니다. 이러한 임시 컬렉션 하위 API 간에는 일관성이 거의 없었기 때문에 처음부터 하나씩 배워야 했고 사용할 때 실수하기 쉬웠습니다. 표준 collection 인터페이스가 등장하면서 문제가 사라졌습니다.
* 새로운 API 설계 노력 감소: 디자이너와 구현자는 컬렉션에 의존하는 API를 만들 때마다 바퀴를 재발명할 필요가 없습니다. 대신 표준 컬렉션 인터페이스를 사용할 수 있습니다.
* 소프트웨어 재사용 촉진: 표준 컬렉션 인터페이스를 준수하는 새로운 데이터 구조는 본질적으로 재사용이 가능합니다. 이러한 인터페이스를 구현하는 객체에서 작동하는 새로운 알고리즘도 마찬가지입니다.





## ArrayList VS Vector

| point           | ArrayList                            | Vector                           |
| --------------- | ------------------------------------ | -------------------------------- |
| synchronization | no                                   | synchronized                     |
| size growth     | increases only by half of its length | doubles its size                 |
| iteration       | can only use Iterator                | can use iterator and Enumeration |
| performance     | faster                               | slower(due to synchronization)   |
| framework       | Collections                          | legacy class                     |



## Arrays VS Collection

| No. |                              Arrays                             |                    Collection Framework                   |
| --- | :-------------------------------------------------------------: | :-------------------------------------------------------: |
| 1   |      배열은 크기가 고정되어 있으므로 일단 배열을 만들면 요구 사항에 따라 늘리거나 줄일 수 없습니다.     | 컬렉션은 우리의 요구 사항에 따라 본질적으로 확장 할 수 있습니다. 크기를 늘리거나 줄일 수 있습니다. |
| 2   | 기본 유형(boolean, byte, short, int, long 등)과 객체 유형을 모두 보유할 수 있습니다. |                      객체 유형만 보유할 수 있음                      |
| 3   |        배열에는 기본 데이터 구조가 없습니다. 자바에서 데이터 구조로 사용되는 배열 자체입니다.        |            모든 Collection 클래스에는 기본 데이터 구조가 있습니다.           |
| 4   |                       배열에는 유틸리티 메서드가 없습니다                       |              모든 Collection은 유틸리티 메서드를 제공합니다.              |



### Why Collection ?

1. 배열은 크기가 고정되어 있으며 개발 단계에서는 항상 배열의 크기를 정의할 수 없습니다. 요구 사항에 따라 크기를 늘려야 합니다. 컬렉션은 크기가 고정되어 있지 않고 확장 가능합니다(최대 크기에 도달하면 크기가 증가함).
2. 배열은 한 가지 유형의 요소만 보유할 수 있습니다. 컬렉션은 다양한 유형의 요소를 보유할 수 있습니다.
3. 정렬, 검색 등을 위한 배열의 기본 메서드 지원은 없지만 컬렉션에는 기본 유틸리티 메서드 지원이 있어 개발자가 편리하게 사용할 수 있습니다. 코딩 시간이 단축됩니다.



## 시간 복잡도

### ArrayList VS LinkedList

| 비교 목록                               | ArrayList                 | LinkedList |
| ----------------------------------- | ------------------------- | ---------- |
| Add/remove element in the beginning | O(n)                      | O(1)       |
| Add/remove element in the middle    | O(n)                      | O(1)       |
| Add/remove element in the end       | O(1)                      | O(1)       |
| Get i-th element (random access)    | O(1)                      | O(n)       |
| Find element                        | O(n), O(log(n)) if sorted | O(n)       |
| Traversal order                     | 삽입된 대로                    | 삽입된 대로     |

### Set의 구현체

| 비교 목록           | HashSet            | LinkedHashSet  | TreeSet          | EnumSet           |
| --------------- | ------------------ | -------------- | ---------------- | ----------------- |
| Add element     | amoritize O(1)     | amoritize O(1) | O(log(n))        | O(1)              |
| Remove element  | amoritize O(1)     | amoritize O(1) | O(log(n))        | O(1)              |
| Find Element    | O(1)               | O(1)           | O(log(n))        | O(1)              |
| Traversal order | 무작위, 해시 함수에 의해 흩어짐 | 삽입된 대로         | 요소 비교 기준에 따라 정렬됨 | enum 값의 정의 순서에 따라 |

### Map의 구현체

| 비교 목록           | HashMap            | LinkedHashMap  | TreeMap          | EnumMap           |
| --------------- | ------------------ | -------------- | ---------------- | ----------------- |
| Add element     | amoritize O(1)     | amoritize O(1) | O(log(n))        | O(1)              |
| Remove element  | amoritize O(1)     | amoritize O(1) | O(log(n))        | O(1)              |
| Find element    | O(1)               | O(1)           | O(log(n))        | O(1)              |
| Traversal order | 무작위, 해시 함수에 의해 흩어짐 | 삽입된 대로         | 요소 비교 기준에 따라 정렬됨 | enum 값의 정의 순서에 따라 |

##

## Reference

> [https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#collection](https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#collection)
>
> [https://www.baeldung.com/java-arraylist-vs-vector](https://www.baeldung.com/java-arraylist-vs-vector)
>
> [https://www.geeksforgeeks.org/difference-between-arrays-and-collection-in-java/](https://www.geeksforgeeks.org/difference-between-arrays-and-collection-in-java/)
>
> [https://en.wikipedia.org/wiki/Data\_structure](https://en.wikipedia.org/wiki/Data\_structure)
>
> [https://www.baeldung.com/java-choose-list-set-queue-map](https://www.baeldung.com/java-choose-list-set-queue-map)
>
> [https://docs.oracle.com/javase/tutorial/collections/intro/index.html](https://docs.oracle.com/javase/tutorial/collections/intro/index.html)
