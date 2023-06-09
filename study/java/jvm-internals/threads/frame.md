# Frame

모든 메서드 호출에 대해 새 frame이 생성되고 stack 맨 위에 추가(푸시)됩니다. 메서드가 정상적으로 반환되거나 메서드 호출 중에 포착되지 않은 예외가 발생하면 frame이 제거(팝)됩니다. 예외 처리에 대한 자세한 내용은 아래 예외 테이블 섹션을 참조하세요.

## Each frame contains:

* Local variable array - 지역 변수 배열
* Return value - 반환 값
* Operand stack - 피연산자 스택
* 현재 메서드의 클래스에 대한 런타임 상수 풀에 대한 참조



## Local Vairable Array

로컬 변수의 배열에는 `this`에 대한 참조, 모든 메서드 매개 변수 및 기타 로컬로 정의된 변수를 포함하여 메서드 실행 중에 사용되는 모든 변수가 포함됩니다. 클래스 메소드(즉, 정적 메소드)의 경우 메소드 매개변수는 0부터 시작하지만 인스턴스 메소드의 경우 `this` 위해 0 슬롯이 예약되어 있습니다.



지역 변수는 다음과 같을 수 있습니다

* `boolean`
* `byte`
* `char`
* `long`
* `short`
* `int`
* `float`
* `double`
* reference
* returnAddress

`long` 및 `double`을 제외하고 모든 유형은 지역 변수 배열에서 단일 슬롯을 사용합니다. `long` 및 `double`은 두 배 너비(32비트 대신 64비트)이기 때문에 둘 다 두 개의 연속 슬롯을 사용합니다.



## Operand Stack <a href="#operand_stack" id="operand_stack"></a>

피연산자 stack은 범용 레지스터가 native CPU에서 사용되는 것과 유사한 방식으로 바이트 코드 명령어 실행 중에 사용됩니다. 대부분의 JVM 바이트 코드는 값을 생성하거나 소비하는 작업을 푸시, 팝핑, 복제, 교환 또는 실행하여 피연산자 stack을 조작하는 데 시간을 보냅니다. 따라서 로컬 변수 배열과 피연산자 스택 간에 값을 이동하는 명령은 바이트 코드에서 매우 자주 사용됩니다. 예를 들어 간단한 변수 초기화로 인해 피연산자 스택과 상호 작용하는 두 개의 바이트 코드가 생성됩니다.

```java
int i;
```

다음 바이트 코드로 컴파일됩니다.

{% code lineNumbers="true" %}
```c
iconst_0      // 피연산자 stack 맨 위로 0 푸시
istore_1      // 피연산자 stack 맨 위에서 값을 팝하고 로컬 변수 1로 저장
```
{% endcode %}

로컬 변수 배열, 피연산자 스택 및 런타임 상수 풀 간의 상호 작용에 대한 자세한 설명은 아래 클래스 파일 구조 섹션을 참조합니다.



## Dynamic Linking <a href="#dynamic_linking" id="dynamic_linking"></a>

각 프레임에는 런타임 상수 풀에 대한 참조가 포함됩니다. 참조는 해당 frame에 대해 실행 중인 메서드의 클래스에 대한 상수 풀을 가리킵니다. 이 참조는 동적 연결을 지원하는 데 도움이 됩니다.



C/C++ 코드는 일반적으로 객체 파일로 컴파일된 다음 여러 객체 파일이 함께 연결되어 실행 파일이나 dll과 같은 사용 가능한 아티팩트를 생성합니다. 연결 단계 동안 각 객체 파일의 symbolic 참조는 최종 실행 파일에 상대적인 실제 메모리 주소로 대체됩니다. Java에서 이 연결 단계는 런타임에 동적으로 수행됩니다.



Java 클래스가 컴파일되면 변수 및 메소드에 대한 모든 참조가 symbolic 참조로 클래스의 상수 풀에 저장됩니다. symbolic 참조는 실제 메모리 위치를 실제로 가리키는 참조가 아닌 논리적 참조입니다. JVM 구현은 symbolic 참조를 확인할 시기를 선택할 수 있습니다. 이것은 로드된 후 클래스 파일이 확인될 때 발생할 수 있으며 eager 또는 static resolution이라고 합니다. 대신 symbolic 참조가 처음으로 사용되는 경우 lazy or late resolution이라고 합니다. 그러나 JVM은 각 참조가 처음 사용될 때 resolution이 발생한 것처럼 동작해야 하며 이 시점에서 resolution 오류를 발생시켜야 합니다. 바인딩은 symbolic 참조로 식별되는 필드, 메서드 또는 클래스가 직접 참조로 대체되는 프로세스이며 symbolic 참조가 완전히 대체되기 때문에 한 번만 발생합니다. symbolic 참조가 아직 해결되지 않은 클래스를 참조하는 경우 이 클래스가 로드됩니다. 각 직접 참조는 변수 또는 메서드의 런타임 위치와 관련된 저장 구조에 대한 오프셋으로 저장됩니다.
