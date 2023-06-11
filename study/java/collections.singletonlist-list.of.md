# Collections.singletonList() vs List.of()

\
`Collections.singletonList()`와 `List.of()`는 둘 다 Java에서 단일 요소를 포함하는 불변(immutable) 리스트를 생성하는 메서드입니다. 하지만 몇 가지 차이점이 있습니다.

1. 가변성: `Collections.singletonList()`로 생성된 리스트는 변경할 수 없습니다. 요소를 추가, 삭제 또는 변경할 수 없습니다. 반면에 `List.of()`로 생성된 리스트는 역시 변경할 수 없지만, 생성 후에 `List` 인터페이스의 다른 메서드를 사용하여 리스트를 수정할 수 있습니다. 예를 들어, `List.of()`로 생성된 리스트의 요소를 `set()` 메서드를 사용하여 변경할 수 있습니다.
2. Null 요소: `Collections.singletonList()`는 요소로 `null`을 허용합니다. 따라서 `Collections.singletonList(null)`와 같이 `null`을 포함하는 리스트를 생성할 수 있습니다. 반면에 `List.of()`는 요소로 `null`을 허용하지 않습니다. `null`을 전달하면 `NullPointerException`이 발생합니다.
3. 타입 안전성: `List.of()`는 가변 인수(varargs)를 사용하여 요소를 전달합니다. 이는 컴파일러가 요소의 타입을 체크할 수 있게 해줍니다. 따라서 컴파일 시점에서 타입 안전성을 보장할 수 있습니다. `Collections.singletonList()`는 단일 요소를 인자로 받기 때문에 컴파일러가 타입 체크를 수행하지 않습니다.
4. 구현 차이: `Collections.singletonList()`는 `SingletonList` 클래스의 인스턴스를 반환합니다. 이 클래스는 `List` 인터페이스를 구현한 클래스입니다. `List.of()`는 `List` 인터페이스의 정적 팩토리 메서드로서, 내부적으로 변경 불가능한 리스트를 구현한 클래스의 인스턴스를 반환합니다.

요약하면, `Collections.singletonList()`와 `List.of()`는 둘 다 단일 요소를 포함하는 변경 불가능한 리스트를 생성하는 메서드입니다. 그러나 `List.of()`는 더 타입 안전하며 `null`을 허용하지 않습니다. 또한, `List.of()`로 생성된 리스트는 생성 후에 수정할 수 있습니다.
