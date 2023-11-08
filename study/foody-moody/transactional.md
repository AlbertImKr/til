# Transactional

이 어노테이션을 사용하면 메서드가 **트랜잭션 경계**에 포함되어야 함을 나타냅니다. Spring은 이 어노테이션을 감지하고 해당 메서드가 **호출**될 때 트랜잭션을 **시작**하며, 메서드가 **종료**될 때 트랜잭션을 **커밋**하거나 **롤백**합니다.

## 주요 특징 및 주의 사항

## **Propaging(전파 옵션):**

* 트랜잭션의 전파 동작을 지정한다

#### 종류

### **1. `REQUIRED` (필요한 경우)**:

*   트랜잭션이 이미 존재하면 그것을 사용하고, 그렇지 않으면 새로운 트랜잭션을 시작합니다.

    ```java
    @Transactional(propagation = Propagation.REQUIRED)
    public void requiredMethod() {
        // 이미 트랜잭션이 존재하면 사용하고, 없으면 새로운 트랜잭션 시작
    }
    ```

### **2.`SUPPORTS` (지원하는 경우)**:

*   트랜잭션이 이미 존재하면 그것을 사용하고, 그렇지 않으면 트랜잭션 없이 실행됩니다.

    ```java
    @Transactional(propagation = Propagation.SUPPORTS)
    public void supportsMethod() {
        // 이미 트랜잭션이 존재하면 사용하고, 없으면 트랜잭션 없이 실행
    }
    ```

### **3. `MANDATORY` (필수)**:

*   트랜잭션이 이미 존재해야만 합니다. 그렇지 않으면 예외가 발생합니다.

    ```java
    @Transactional(propagation = Propagation.MANDATORY)
    public void mandatoryMethod() {
        // 트랜잭션이 반드시 존재해야 함. 없으면 예외 발생
    }
    ```

### **4. `REQUIRES_NEW` (항상 새로운 트랜잭션)**:

*   항상 새로운 트랜잭션을 시작합니다. 이미 존재하는 트랜잭션은 일시 중단됩니다.

    ```java
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void requiresNewMethod() {
        // 항상 새로운 트랜잭션 시작. 기존 트랜잭션은 일시 중단
    }
    ```

### **5. `NOT_SUPPORTED` (트랜잭션 지원 안 함)**:

*   트랜잭션을 지원하지 않아서 트랜잭션 외부에서 실행됩니다. 이미 존재하는 트랜잭션은 일시 중단됩니다.

    ```java
    @Transactional(propagation = Propagation.NOT_SUPPORTED)
    public void notSupportedMethod() {
        // 트랜잭션 없이 실행. 기존 트랜잭션은 일시 중단
    }
    ```

### **6. `NEVER` (트랜잭션 사용 금지)**:

*   트랜잭션을 사용해서는 안 되며, 트랜잭션이 이미 존재하면 예외가 발생합니다.

    ```java
    @Transactional(propagation = Propagation.NEVER)
    public void neverMethod() {
        // 트랜잭션을 사용해서는 안 됨. 이미 존재하면 예외 발생
    }
    ```

### **7. `NESTED` (중첩된 트랜잭션)**:

*   현재 트랜잭션이 존재하면 중첩 트랜잭션을 시작하고, 그렇지 않으면 일반 트랜잭션을 시작합니다.

    ```java
    @Transactional(propagation = Propagation.NESTED)
    public void nestedMethod() {
        // 현재 트랜잭션이 존재하면 중첩 트랜잭션 시작, 그렇지 않으면 일반 트랜잭션 시작
    }
    ```

## **Isolation(격리 수준):**

* 트랜잭션의 격리 수준을 지정한다

#### 종류

1.  **`READ_UNCOMMITTED` (미확인 읽기)**:

    * 문제점: **Dirty Read** (더러운 읽기)
    * 다른 트랜잭션에서 아직 커밋되지 않은 변경 내용을 읽을 수 있습니다.

    ```java
    @Transactional(isolation = Isolation.READ_UNCOMMITTED)
    public void readUncommittedMethod() {
        // 다른 트랜잭션의 커밋되지 않은 변경 내용을 읽을 수 있음
    }
    ```

    * 예시:
      * A 사용자가 10000원을 은행 계좌에 입금하는 트랜잭션을 시작했지만 아직 커밋하지 않았습니다. 이 때, B 사용자가 계좌 잔액을 조회하면 10000원이 더해진 잔액을 볼 수 있습니다. 만약 A 사용자가 트랜잭션을 롤백하면 B 사용자는 잘못된 정보를 본 것이 됩니다.
2.  **`READ_COMMITTED` (확인 읽기)**:

    * 문제점: **Non-Repeatable Read** (비반복 읽기)
    * 다른 트랜잭션에서 커밋된 변경 내용만 읽을 수 있습니다.

    ```java
    @Transactional(isolation = Isolation.READ_COMMITTED)
    public void readCommittedMethod() {
        // 다른 트랜잭션의 커밋된 변경 내용만 읽을 수 있음
    }
    ```

    * 예시:
      * A 사용자가 자신의 계좌 잔액을 조회했을 때 50000원이 나옵니다. 그 사이 B 사용자가 A 사용자의 계좌에서 20000원을 출금했습니다. A 사용자가 다시 잔액을 조회하면 30000원이 나옵니다. 같은 트랜잭션 내에서 두 번의 조회 결과가 다릅니다.
3.  **`REPEATABLE_READ` (반복 가능한 읽기)**:

    * 문제점: **Phantom Read** (유령 읽기)
    * 트랜잭션이 시작할 때 읽은 데이터는 해당 트랜잭션 동안 다른 트랜잭션에 의해 변경되지 않습니다.

    ```java
    @Transactional(isolation = Isolation.REPEATABLE_READ)
    public void repeatableReadMethod() {
        // 트랜잭션 동안 읽은 데이터는 다른 트랜잭션에 의해 변경되지 않음
    }
    ```

    * 예시:
      * A 사용자가 30세 이하의 고객을 조회했을 때 5명이 나옵니다. 그 사이 B 사용자가 새로운 30세 이하의 고객을 추가했습니다. A 사용자가 다시 30세 이하의 고객을 조회하면 6명이 나옵니다.
4.  **`SERIALIZABLE` (직렬화 가능)**:

    * 트랜잭션들이 순차적으로 실행되어 동시성 문제가 발생하지 않도록 합니다.

    ```java
    @Transactional(isolation = Isolation.SERIALIZABLE)
    public void serializableMethod() {
        // 코드
    }
    ```

    * 예시:
      * A 사용자가 자신의 계좌 잔액을 변경하는 트랜잭션을 시작하면, 그 동안 B 사용자는 A 사용자의 계좌에 어떠한 작업도 수행할 수 없습니다. A 사용자의 트랜잭션이 완료될 때까지 기다려야 합니다

## 동시성 문제

1. **Dirty Reads (오염된 읽기)**:
   * 한 트랜잭션에서 변경된 (그러나 아직 커밋되지 않은) 데이터를 다른 트랜잭션에서 읽는 경우 발생합니다. 이로 인해 아직 확정되지 않은 데이터를 기반으로 작업을 수행할 수 있습니다.
   * **해결방법**:
     * **`READ_COMMITTED`** 이상의 격리 수준을 사용하여 Dirty Reads를 방지할 수 있습니다. 이렇게 하면 커밋된 데이터만 읽게 됩니다.
2. **Non-Repeatable Reads (비반복 가능한 읽기)**:
   * 한 트랜잭션 도중에 같은 데이터를 두 번 이상 읽을 때, 두 번째 읽기에서 첫 번째 읽기와 다른 값을 얻는 경우 발생합니다. 이는 다른 트랜잭션에 의해 해당 데이터가 변경되었기 때문입니다.
   * **해결방법**:
     * **`REPEATABLE_READ`** 이상의 격리 수준을 사용하여 Non-Repeatable Reads를 방지할 수 있습니다. 트랜잭션이 시작될 때 읽은 데이터는 해당 트랜잭션 동안 변경되지 않도록 보장됩니다.
3. **Phantom Reads (환영 읽기)**:
   * 한 트랜잭션 도중에 특정 조건을 기반으로 데이터를 두 번 이상 읽을 때, 두 번째 읽기에서 첫 번째 읽기에서 보았던 데이터의 수와 다르게 나타나는 경우 발생합니다. 이는 다른 트랜잭션에 의해 새로운 데이터가 추가되거나 삭제되었기 때문입니다.
   * **해결방법**:
     * **`SERIALIZABLE`** 격리 수준을 사용하여 Phantom Reads를 방지할 수 있습니다. 트랜잭션들이 순차적으로 실행되어, 한 트랜잭션이 실행되는 동안 다른 트랜잭션에서 데이터의 추가나 삭제가 발생하지 않도록 합니다.

## **Timeout (시간 초과)**:

*   **`timeout`** 옵션은 해당 트랜잭션이 지정된 시간 내에 완료되지 않으면 강제로 롤백됩니다.

    ```java
    @Transactional(timeout = 5)  // 5초
    public void timeoutMethod() {
        // 이 트랜잭션은 5초 내에 완료되지 않으면 롤백됩니다.
    }
    ```

## **Read-only (읽기 전용)**:

*   **`readOnly`** 옵션은 해당 트랜잭션 내에서 데이터 변경 작업이 발생하면 예외가 발생합니다. 읽기 전용 작업에서는 성능 향상이 있을 수 있습니다.

    ```java
    @Transactional(readOnly = true)
    public Account getAccount(Long id) {
        // 이 메서드는 데이터를 읽기만 해야 합니다. 데이터 수정은 하면 안 됩니다.
        return accountRepository.findById(id).orElse(null);
    }
    ```

### 성능 향상 이유:

1. **최적화된 데이터베이스 엔진 동작**:
   * 대부분의 RDBMS는 여러 유형의 락킹 메커니즘을 제공합니다. **읽기 전용 트랜잭션**의 경우, 데이터베이스는 데이터 변경에 **관련된 락(예: 쓰기 락)을 획득**하는 것을 방지할 수 있습니다. 이로 인해 **읽기 전용 트랜잭션**은 다른 트랜잭션에 의해 **블로킹되는 빈도가 줄어**들 수 있습니다.
   * **예**: 상점의 재고 확인 시스템에서, 여러 사용자가 동시에 재고를 확인하는 경우에, 읽기 전용 트랜잭션을 사용하면 데이터베이스에서 재고 정보를 빠르게 가져올 수 있습니다. 데이터 변경이 없기 때문에, 쓰기 락 없이 정보를 조회할 수 있어, 블로킹 없이 동시에 여러 사용자의 요청을 처리할 수 있습니다.
2. **버전 관리와 락킹 최소화**:
   * ORM 도구(예: Hibernate)는 데이터 변경을 추적하기 위해 내부적으로 **버전 관리**를 사용할 수 있습니다. 이는 데이터베이스 행에 대한 **버전 번호**를 사용하여, 두 사용자가 동시에 같은 데이터를 수정하는 **상황(동시성 문제)을 해결**하는데 도움을 줍니다. 그러나 읽기 전용 트랜잭션에서는 데이터 변경이 없으므로, 이러한 **버전 관리**가 필요 없어집니다.
   * **예**: 은행의 계좌 잔액 확인 시스템에서, 사용자가 잔액만 확인하려는 경우, 읽기 전용 트랜잭션을 사용하면 ORM이 버전 관리나 락킹 메커니즘을 적용하지 않아도 되므로, 응답 속도가 향상될 수 있습니다.
3. **캐싱 효율성**:
   * ORM 프레임워크는 종종 **쿼리 결과**를 **캐시**하여, 동일한 쿼리에 대해 빠른 응답 시간을 제공합니다. 읽기 전용 트랜잭션에서는 데이터 변경이 없기 때문에, 캐시된 데이터의 유효성에 대한 우려가 줄어듭니다.
   * **예**: 뉴스 웹사이트에서, 사용자가 특정 카테고리의 뉴스 목록을 조회하는 경우, 읽기 전용 트랜잭션을 사용하면 ORM 프레임워크는 이전에 조회된 결과를 캐시에서 빠르게 가져올 수 있습니다.
4. **일관성 유지**:
   * 읽기 전용 트랜잭션은 **실수**로 데이터 **변경**을 **방지**합니다. 의도치 않게 데이터를 변경하는 것은 성능 문제뿐만 아니라 데이터 **무결성** 문제를 야기할 수 있습니다.
   * **예**: 병원 시스템에서, 환자의 병력을 확인하는 경우, 읽기 전용 트랜잭션을 사용하면, 실수로 환자 정보를 변경하는 것을 방지할 수 있습니다.

## **Rollback rules (롤백 규칙)**:

* 기본적으로 런타임 예외(**`RuntimeException`** 및 그 하위 예외)가 발생하면 롤백을 수행합니다
*   **`rollbackFor`**

    * 이 속성을 사용하면 특정 예외가 발생했을 때 트랜잭션을 롤백하는 규칙을 지정할 수 있습니다.

    ```java
    @Transactional(rollbackFor = InsufficientBalanceException.class)
    public void transferMoney() {
        // InsufficientBalanceException이 발생하면 트랜잭션은 롤백됩니다.
    }
    ```
*   **`noRollbackFor`**

    * 이 속성은 지정된 예외 유형에 대한 롤백을 방지합니다. 즉, 해당 예외가 발생하더라도 트랜잭션은 롤백되지 않습니다.

    ```java
    @Transactional(noRollbackFor = BusinessException.class)
    public void businessOperation() {
        // ...
        // BusinessException이 발생해도 트랜잭션은 롤백되지 않습니다.
    }
    ```

## **AOP (관점 지향 프로그래밍)**:

*   내부 메서드 호출에서는 \*\*`@Transactional`\*\*이 제대로 동작하지 않는 문제는 Spring의 프록시 방식 때문입니다. 해당 문제를 해결하기 위해서는 메서드 호출을 외부 빈을 통해 간접적으로 수행하거나, 프로그램 구조를 변경해야 합니다.

    ```java
    @Service
    public class MyService {
        @Transactional
        public void methodA() {
            // ...
            this.methodB();  // 주의: methodB의 트랜잭션 로직이 제대로 적용되지 않을 수 있습니다!
        }
        
        @Transactional
        public void methodB() {
            // ...
        }
    }
    ```
