# Spring Boot Project 4주차 회고

##

## 1. Identifiers in Hibernate/JPA

{% content-ref url="../db/identifiers-in-hibernate-jpa.md" %}
[identifiers-in-hibernate-jpa.md](../db/identifiers-in-hibernate-jpa.md)
{% endcontent-ref %}

## 2. Logging Level

{% content-ref url="../java/logging-level.md" %}
[logging-level.md](../java/logging-level.md)
{% endcontent-ref %}

## 3. Filter,Interceptor,AOP,Argument Resolver

{% content-ref url="../spring/filter-interceptor-aop-argument-resolver.md" %}
[filter-interceptor-aop-argument-resolver.md](../spring/filter-interceptor-aop-argument-resolver.md)
{% endcontent-ref %}

## 4. Gradle에서 종속성(Dependency)을 관리

{% content-ref url="../java/manage-dependencies-in-gradle.md" %}
[manage-dependencies-in-gradle.md](../java/manage-dependencies-in-gradle.md)
{% endcontent-ref %}

## 5. Mock 과 Stub

\
Mock과 Stub은 테스트 더블(Test Double)의 일종으로, 테스트 환경에서 독립적으로 실행 가능한 가짜 객체를 생성하는 데 사용됩니다. 각각의 사용 시기는 다음과 같습니다:

Mock:

* 객체 간의 상호작용을 테스트하고 검증하기 위해 주로 사용됩니다.
* 객체 간의 메서드 호출, 상태 변경, 예외 발생 등을 검증할 수 있습니다.
* 실제 객체의 동작을 흉내내기 때문에, 호출되어야 하는 메서드와 호출 횟수, 매개변수 등을 정확히 검증할 수 있습니다.
* 특정 메서드 호출에 대한 반환 값을 정의하지 않습니다. 대신, 예상된 메서드 호출이 이루어졌는지 검증합니다.

Stub:

* 특정 메서드 호출에 대한 예상된 반환 값을 정의하기 위해 사용됩니다.
* 실제 객체의 동작을 대체하여 원하는 결과를 반환합니다.
* 호출된 메서드에 대해 고정된 반환 값을 제공하거나, 입력에 따라 다른 결과를 반환하는 등의 동작을 정의할 수 있습니다.
* 객체 간의 상호작용을 검증하는 것보다는, 메서드 호출 시 올바른 반환 값을 받는지 확인하는 데 더 중점을 둡니다.

일반적으로 Mock은 객체 간의 상호작용을 검증하고 Stub은 메서드 호출에 대한 예상된 반환 값을 정의하는 데 사용됩니다. Mock은 객체 간의 상호작용을 중심으로 검증하며, Stub은 메서드 호출의 반환 값을 조작하여 특정 시나리오를 재현하는 데 사용됩니다.

하지만 Mock과 Stub은 상황에 따라 유연하게 사용될 수 있으므로 엄격한 규칙은 없습니다. 테스트의 목적과 필요에 따라 Mock과 Stub을 조합하여 사용할 수도 있습니다. 중요한 것은 테스트 시나리오에 맞게 올바른 도구를 선택하고, 목적에 맞게 Mock과 Stub을 활용하는 것입니다.

## 6. 모킹(Mocking) 프레임워크에서의 `given`과 `when`

`given`과 `when`의 주요한 차이점은 다음과 같습니다.

1. `given`: 모의 객체의 상태나 동작을 설정하는 데 사용됩니다. 특정 메서드 호출 또는 상황에서 모의 객체가 어떻게 동작해야 하는지를 정의합니다. 일반적으로 `given` 절에서는 `when` 절에서 발생할 것으로 예상되는 메서드 호출에 대한 응답을 설정합니다.
2. `when`: 실제 테스트 동작을 수행하는 데 사용됩니다. 모의 객체를 호출하고 예상한 동작이 발생하는지 확인합니다. `when` 절은 테스트 동작의 일부로 실제로 실행되는 코드를 포함합니다.

간단한 예를 통해 설명하겠습니다. 가정해봅시다.

```java
javaCopy codepublic interface MyService {
    int calculate(int a, int b);
}

// 모의 객체 생성
MyService myServiceMock = Mockito.mock(MyService.class);

// given
Mockito.when(myServiceMock.calculate(2, 3)).thenReturn(5);

// when
int result = myServiceMock.calculate(2, 3);

// 결과 확인
System.out.println(result); // 5
```

위의 예시에서 `given` 절에서 `calculate(2, 3)` 메서드가 호출되었을 때 반환할 값을 설정했습니다. 그리고 `when` 절에서 실제로 `calculate(2, 3)` 메서드를 호출하고 반환된 값을 `result` 변수에 저장했습니다. 마지막으로 `result` 값을 확인하여 모의 객체의 동작이 예상대로 수행되는지 확인했습니다.

요약하자면, `given`은 모의 객체의 동작을 설정하고, `when`은 실제 동작을 수행하여 테스트 결과를 확인하는 데 사용됩니다.

\
