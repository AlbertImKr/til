# Generic

제네릭은 자바에서 안정성을 맡고 있다고 할 수 있다. 다양한 객체들을 다루는 메서드나 컬렉션 클래스에서 사용하는 것으로, 컴파일 과정에서 타입체크를 해주는 기능이다. 객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안전성을 높이고 형변환의 번거로움을 줄여준다.  자연스럽게 코드도 더 간결해진다. 예를 들면,Collection 에 특정 객체만 추가될 수 있도록, 또는 특정한 클래스의 특징을 갖고 있는 경우에만 추가될 수 있도록 하는 것이 제네릭이다. 이로 인한 장점은 collection 내부에서 들어온 값이 내가 원하는 값인지 별도의 로직처리를 구현할 필요가 없어진다. 또한 api를 설계하는데 있어서 보다 명확한 의사전달이 가능해진다.



## Why Use Generics?

간단히 말해서 제네릭은 클래스, 인터페이스 및 메서드를 정의할 때 형식(클래스 및 인터페이스)이 매개 변수가 되도록 합니다. 메소드 선언에 사용되는 보다 친숙한 형식 매개변수와 마찬가지로 타입 매개변수는 입력이 다른 동일한 코드를 재사용할 수 있는 방법을 제공합니다. 형식 매개변수에 대한 입력은 값인 반면  타입 매개변수에 대한 입력은 타입이라는 차이점이 있다.

### 제네릭을 사용하는 코드는 제네릭이 아닌 코드에 비해 많은 이점이 있습니다.

#### 1. 컴파일 타임에 더 강력한 유형 검사

Java 컴파일러는 일반 코드에 강력한 타입 검사를 적용하고 코드가 타입 안전을 위반하는 경우 오류를 발생시킵니다. 컴파일 타임 오류를 수정하는 것이 찾기 어려울 수 있는 런타임 오류를 수정하는 것보다 쉽습니다.

#### 2. casts 제거

제네릭이 없는 다음 코드 스니펫은 casting이 필요합니다.&#x20;

```java
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);
```

제네릭이 사용하도록 다시 작성할 때 코드는 캐스팅이 필요하지 않습니다.

```java
List<String> list = new ArrayList<String>();
list.add("hello");
String s = list.get(0);   // no cast
```

#### 3. 프로그래머가 일반 알고리즘을 구현할 수 있도록 합니다.

제네릭을 사용하여 프로그래머는 다양한 유형의 컬렉션에서 작동하고 사용자 정의할 수 있으며 유형이 안전하고 읽기 쉬운 제네릭 알고리즘을 구현할 수 있습니다.



## Generic Types



제네릭 타입은 타입에 대해 매개 변수화되는 제네릭 클래스 또는 인터페이스이다.&#x20;

### A Generic Version of the Box Class

```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

### Type Parameter Naming Conventions

The most commonly used type parameter names are:

* E - Element (used extensively by the Java Collections Framework)
* K - Key
* N - Number
* T - Type
* V - Value
* S,U,V etc. - 2nd, 3rd, 4th types



### Multiple Type Parameters Example



```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
```

> [https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#generic](https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#generic)
>
> [https://docs.oracle.com/javase/tutorial/java/generics/types.html](https://docs.oracle.com/javase/tutorial/java/generics/types.html)
>
> [https://docs.oracle.com/javase/tutorial/java/generics/why.html](https://docs.oracle.com/javase/tutorial/java/generics/why.html)
