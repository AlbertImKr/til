# Threads

스레드는 프로그램에서 실행되는 스레드입니다. JVM을 사용하면 애플리케이션이 동시에 실행되는 여러 실행 스레드를 가질 수 있습니다. Hotspot JVM에는 Java 스레드와 기본 운영 체제 스레드 간에 직접 매핑이 있습니다. Thread-local storage, allocation buffers, synchronization objects, stacks 및 program counter와 native thread가 생성됩니다. The native thread는 Java thread가 종료되면 회수됩니다. 따라서 운영 체제는 모든 thread를 scheduling하고 사용 가능한 CPU로 보내는 책임이 있다. The native thread가 초기화되면 Java thread에서 run() 메서드를 호출합니다. run() 메서드가 반환되면 포착되지 않은 예외가 처리되고 native thread는 thread 종료의 결과로 JVM을 종료해야 하는지 확인합니다(즉, 마지막 non-deamon thread인지). Thread가 종료되면 native thread와 Java thread 모두에 대한 모든 리소스가 해제됩니다.&#x20;

## JVM System Threads

jconsole 혹은 any debugger를 사용하는 경우 백그라운드에서 실행 중인 수많은 threads가 있음을 알 수 있습니다. 이러한 백그라운드 threads는 `purblic static void main(String[])` 호출한 part로 생성되는 main thread 및 main thread에 의해 생성된 모든 thread와 함께 실행됩니다. Hotspot JVM의 기본 백그라운드 시스템 스레드는 다음과 같습니다.

<table><thead><tr><th width="190">Threads</th><th>Description</th></tr></thead><tbody><tr><td>VM thread</td><td>이 스레드는 JVM이 safe-point에 도달해야 하는 작업이 나타날 때까지 기다립니다.   이러한 작업이 별도의 thread에서 발생해야 하는 이유는 모든 작업이 JVM이 heap에 대한 수정이 발생할 수 없는 safe point에 있어야 하기 때문입니다.  이 스레드가 수행하는 작업 유형은 "stop-the-world" 이 스레드는 주기적 작업의 실행을 schedule하는 데 사용되는 타이머 이벤트(예: 인터럽트)를 담당합니다.garbage collections, thread stack dumps, thread suspension 및 biased locking revocation입니다.</td></tr><tr><td>Periodic task thread</td><td>이 스레드는 주기적 작업의 실행을 schedule하는 데 사용되는 timer events(예: interrupts)를 담당합니다.</td></tr><tr><td>GC thread</td><td>이러한 스레드는 JVM에서 발생하는 다양한 유형의 garbage collection 활동을 지원합니다.</td></tr><tr><td>Compiler threads</td><td>이러한 스레드는 런타임에 바이트 코드를 네이티브 코드로 컴파일합니다.</td></tr><tr><td>Signal dispatcher thread</td><td>이 스레드는 JVM 프로세스로 전송된 신호를 수신하고 적절한 JVM 메서드를 호출하여 JVM 내부에서 신호를 처리합니다.</td></tr></tbody></table>

## Per Thread

각 실행 스레드에는 다음 구성 요소가 있습니다.

### Program Counter (PC) <a href="#program_counter" id="program_counter"></a>

기본 명령어가 아닌 경우 현재 명령어(또는 opcode)의 주소이다. 현재 메서드가 natice일 때 PC는 정의되지 않습니다.  모든 CPUs에는 PC가 있으며 일반적으로 PC는 각 명령 후에 증가하므로 실행할 다음 명령의 주소를 보유합니다. JVM은 PC를 사용하여 명령을 실행하는 위치를 추적하며 PC는 실제로 메서드 영역의 메모리 주소를 가리킵니다.



### Stack

각 스레드에는 해당 스레드에서 실행되는 각 메서드에 대한 frame을 보유하는 자체 stack이 있습니다. Stack은 LIFO(Last In First Out) 데이터 구조이므로 현제 실행 중인 메서드가 stack의 맨 위에 있습니다. 모든 메서드 호출에 대해 새 frame이 생성되고 stack 맨 위에 추가(푸시)됩니다. 메서드가 정상적으로 반환되거나 메서드 호출 중에 포착되지 않은 예외가 발생하면 frame이 제거(팝)됩니다. stack은 frame 객체를 푸시하고 팝하는 경우를 제외하고는 직접 조작되지 않으므로 frame 객체는 heap에 할당될 수 있으며 메모리는 연속적일 필요가 없습니다.



### Native Stack

모든 JVM이 native 메서드를 지원하는 것은 아니지만 일반적으로 thread 별 native 메서드 스택을 생성합니다. JVM이 JNI(Java Native Invocation)용 C-linkage 모델을 사용하여 구현된 경우 native stack은 C stack이 됩니다. 이 경우 인수 및 반환 값의 순서는 일반적인 C 프로그램의 native stack과 동일합니다. Native 메서드는 일반적으로(JVM 구현에 따라) JVM으로 다시 호출하고 Java 메소드를 호출할 수 있습니다. 이러한 native Java 호출은 stack(일반 Java 스택)에서 발생합니다. Thread는 native stack을 떠나 stack(일반 Java 스택)에 새 frame을 생성합니다.



### Stack Restrictions

Stack은 동적 또는 고정 크기일 수 있습니다.Thread에 허용된 것보다 더 큰 stack 필요한 경우 StackOverflowError가 발생합니다.  Thread에 새 frame이 필요하고 이를 할당할 메모리가 충분하지 않으면 OutOfMemoryError가 발생합니다.



### [Frame](frame.md) <a href="#frame" id="frame"></a>

모든 메서드 호출에 대해 새 frame이 생성되고 stack 맨 위에 추가(푸시)됩니다. 메서드가 정상적으로 반환되거나 메서드 호출 중에 포착되지 않은 예외가 발생하면 frame이 제거(팝)됩니다. 예외 처리에 대한 자세한 내용은 아래 예외 테이블 섹션을 참조하세요.

