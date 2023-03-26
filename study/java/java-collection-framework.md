# Java Collection Framework

Java Collection Framework는 Map과  Collection 인터페이스를 기준으로 여러 구현체가 존재한다. Collection 인터페이스는 List,Set,Queue 등 인터페이스의 상위 인터페이스입니다. 다수의 Data를 다루는데 표준화된 클래스들을 제공해주기 때문에 Data Structure 를 직접 구현하지 않고 편하게 사용할 수 있다. 또한 배열과 다르게 객체를 보관하기 위한 공간을 정하지 않아도 되므로, 상황에 따라 공간을 동적으로 정할 수 있다. 이는 프로그램의 공간적인 효율성을 또한 높여준다.

* List
  * 대표적인 구현체로는 ArrayList가 존재한다. 이는 기존에 있었던 Vector를 개선한 것이다. 이외에도 LinkedList 등의 구현체가 있다.
* Map
  * 대표적인 구현체로 HashMap이 존재한다. key-value의 구조로 이루어져 있다. key는 중복된 값을 저장하지 않으며 순서를 보장하지 않는다. key 에 대해서 순서를 보장하기 위해서는 LinkedHashMap을 사용한다.
* Set
  * 대표적인 구현체로 HashSet가 존재한다. value에 대해서 중복된 값을 저장하지 않는다.  마찬가지로 순서를 보장하지 않으며 순서를 보장해주기 위해서는 LinkedHashSet 을 사용한다.
* Stack 과 Queue
  * Stack 객체는 직접 new 키워드로 사용할 수 있으며, Queue 인터페이스는 JDK 1.5 부터 LinkedList에 new 키워드를 적용하여 사용할 수 있다.&#x20;



## ArrayList vs Vector

| point           | ArrayList                            | Vector                           |
| --------------- | ------------------------------------ | -------------------------------- |
| synchronization | no                                   | synchronized                     |
| size growth     | increases only by half of its length | doubles its size                 |
| iteration       | can only use Iterator                | can use iterator and Enumeration |
| performance     | faster                               | slower(due to synchronization)   |
| framework       | Collections                          | legacy class                     |



## Java Collection Framework

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

##

## Difference between Arrays and Collection

| No. |                              Arrays                             |                    Collection Framework                   |
| --- | :-------------------------------------------------------------: | :-------------------------------------------------------: |
| 1   |      배열은 크기가 고정되어 있으므로 일단 배열을 만들면 요구 사항에 따라 늘리거나 줄일 수 없습니다.     | 컬렉션은 우리의 요구 사항에 따라 본질적으로 확장 할 수 있습니다. 크기를 늘리거나 줄일 수 있습니다. |
| 2   | 기본 유형(boolean, byte, short, int, long 등)과 객체 유형을 모두 보유할 수 있습니다. |                      객체 유형만 보유할 수 있음                      |
| 3   |        배열에는 기본 데이터 구조가 없습니다. 자바에서 데이터 구조로 사용되는 배열 자체입니다.        |            모든 Collection 클래스에는 기본 데이터 구조가 있습니다.           |
| 4   |                       배열에는 유틸리티 메서드가 없습니다                       |              모든 Collection은 유틸리티 메서드를 제공합니다.              |



## Why collections ?

1. 배열은 크기가 고정되어 있으며 개발 단계에서는 항상 배열의 크기를 정의할 수 없습니다. 요구 사항에 따라 크기를 늘려야 합니다. 컬렉션은 크기가 고정되어 있지 않고 확장 가능합니다(최대 크기에 도달하면 크기가 증가함).
2. 배열은 한 가지 유형의 요소만 보유할 수 있습니다. 컬렉션은 다양한 유형의 요소를 보유할 수 있습니다.
3. 정렬, 검색 등을 위한 배열의 기본 메서드 지원은 없지만 컬렉션에는 기본 유틸리티 메서드 지원이 있어 개발자가 편리하게 사용할 수 있습니다. 코딩 시간이 단축됩니다.



## Data structure

일반적으로 데이터에 대한 효율적인 접근을 위해 선택되는 데이터 구성, 관리 및 저장 형식입니다. 보다 정확하게는 데이터 구조는 데이터 값들과 그들 사이의 관계 및 데이터에 적용될 수 있는 기능이나 연산의 집합이다.



> [https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#collection](https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#collection)
>
> [https://www.baeldung.com/java-arraylist-vs-vector](https://www.baeldung.com/java-arraylist-vs-vector)
>
> [https://www.geeksforgeeks.org/difference-between-arrays-and-collection-in-java/](https://www.geeksforgeeks.org/difference-between-arrays-and-collection-in-java/)
>
> [https://en.wikipedia.org/wiki/Data\_structure](https://en.wikipedia.org/wiki/Data\_structure)
