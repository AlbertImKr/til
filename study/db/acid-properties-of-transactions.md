# ACID properties of transactions

## **Atomicity**

* 데이터에 대한 모든 변경은 단일 작업인 것처럼 수행됩니다.
* 즉, 모든 변경 사항이 수행되거나 아무 것도 수행되지 않습니다. 예를 들어, 한 계정에서 다른 계정으로 자금을 이체하는 애플리케이션에서 원자성 속성은 한 계정에서 출금이 성공적으로 이루어지면 해당 계정이 다른 계정으로 입금되도록 합니다.

## **Consistency**

* 데이터는 트랜잭션이 시작될 때와 끝날 때 일관된 상태입니다.
* 예를 들어 한 계좌에서 다른 계좌로 자금을 이체하는 애플리케이션에서 일관성 속성은 각 거래의 시작과 종료 시 두 계좌의 총 자금 가치가 동일하도록 합니다.

## **Isolation**

* 트랜잭션의 중간 상태는 다른 트랜잭션에 보이지 않습니다. 그 결과 동시에 실행되는 트랜잭션이 직렬화되는 것처럼 보입니다.
* 예를 들어 한 계정에서 다른 계정으로 자금을 이체하는 애플리케이션에서 격리 속성은 다른 트랜잭션이 한 계정 또는 다른 계정에서 이체된 자금을 볼 수 있도록 하지만 둘 다에서 볼 수 없도록 합니다.

## Durability

* 트랜잭션이 성공적으로 완료된 후 데이터에 대한 변경 사항은 유지되며 시스템 오류가 발생하더라도 실행 취소되지 않습니다.
* 예를 들어 한 계정에서 다른 계정으로 자금을 이체하는 애플리케이션에서 내구성 속성은 각 계정에 적용된 변경 사항이 취소되지 않도록 합니다

## 트랜잭션 처리

트랜잭션 처리 시스템을 사용하면 애플리케이션 프로그래머가 트랜잭션 관리의 세부 사항으로부터 애플리케이션 프로그램을 보호하여 비즈니스를 지원하는 코드 작성에 집중할 수 있습니다.

* 트랜잭션의 동시 처리를 관리합니다.
* 데이터 공유를 가능하게 합니다.
* 데이터의 무결성을 보장합니다.
* 트랜잭션 실행의 우선 순위를 관리합니다.

{% embed url="https://www.ibm.com/docs/en/cics-ts/5.4?topic=processing-acid-properties-transactions" %}

{% embed url="https://www.ibm.com/docs/en/SSGMCP_5.4.0/product-overview/TransactionProcessing.html" %}

{% embed url="https://www.baeldung.com/java-transactions" %}

{% embed url="https://www.baeldung.com/cs/transactions-intro" %}

{% embed url="https://www.baeldung.com/spring-transactions-read-only#2-transaction-management" %}

