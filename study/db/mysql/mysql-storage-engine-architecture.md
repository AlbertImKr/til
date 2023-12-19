# MySQL Storage Engine Architecture

## [MySQL Storage Engine Architecture(version 8.0)](https://dev.mysql.com/doc/refman/8.0/en/pluggable-storage-overview.html)

!\[\[MysqlStorgeEngineArchitecture.png]]

## MySQL Connectors

* 다양한 프로그래밍 언어와 기술이 MySQ에 연결하는 인터페이스다.
* JDBC와 같은 커넥터 및 Node.js 같은 언어를 위한 커넥터 들이 있다.
* 이 커넥터들은 다양한 언어로 작성된 응용 프로그램이 MySQL 데이터베이스와 사용 작용하고, 쿼리를 수행하고 , 결과를 받을 수 있게 해준다.

## MySQL Shell(Scripting)

* 사용자가 데이터베이스를 관리하고, SQL 쿼리를 실행하며,SQL 및 MySQL이 지원하는 다른 스크립팅 언어로 스크립트를 작성할 수 있게 해주는 명령줄 인터페이스다.
* 사용방법

```javascript
// MySQL 쉘 설치
brew install --cask mysql-shell
// MySQL 쉘 시작
mysqlsh
// JavaScript 모드로 전환
\js
// 데이터베이스에 연결 (\connect albert@localhost:3306/{database})
\connect user@host. 
// sql 전화
\sql 
// SQL 문 실행 가능 
show tables
```

## MySQL 서버 프로세스(mysqld)

**모든 데이터베이스 명령어(SQL 쿼리)를 처리하는 핵심구성 요소다**

### NoSQL 인터페이스 CRUD 작업

* NoSQL 인터페이스를 사용하여 생성(Create),일기(Read),갱신(Update),삭제(Delete) 작업을 수행할 수 있는 방법을 제공한다.

### SQL 인터페이스

* DML(데이터 조작 언어 - INSERT,UPDATE,DELETE,SELECT)과 DDL(데이터 정의 언어 - CREATE,ALTER,DROP)과 같은 표준 SQL 작업을 처리한다.

### 파서

* SQL 쿼리의 구문을 해석하고 검사한다.
* 구문 트리(Syntax tree)를 만든다.
* 이 과정에서 문법 오류 검사가 이루어진다.

### 전처리기(그림에 없지만)

* 쿼리 파서에서 만든 트리를 바탕으로 전처리를 시작한다.
* 테이블이나 컬럼의 존재 여부, 접근 권한 등 의미론적(Semantic) 오류를 검사한다.
* 쿼리 파서와 전처리기는 컴파일 과정과 매우 유사하다.
* 하지만 SQL은 프로그래밍 언어처럼 컴파일 타임에 검증할 수 없기 때문에 매번 구문 평가를 진행한다.

### Optimizer

* SQL 쿼리를 분석하고 가장 효율적인 실행 방법을 결정한다.

### 캐시 및 버퍼

* 데이터 접근 기간을 줄이기 위해 일시적으로 데이터를 저장하는 데 사용도니다. 글로벌 캐시 및 버퍼와 스토리지 엔진별 캐시 및 버퍼가 포함된다.

## 스토리지 엔진(Default)

실제로 데이터를 저장하고 검색하는 구성 요소다. MySQL은 다양한 유형의 작업에 최적화된 여러 소토리지 엔진을 지원한다

### InnoDB

* MySQL의 기본 스토리지 엔진으로, 외래 키 지원과 함께 전체 ACID 준수 트랜잭션 기능을 제공한다.

### MyISAM

* 고속 저장을 제공하지만 트랜잭션 기능이 없는 오래된 스토리지 엔진이다.

### NDB 클러스터

* 높은 중복성과 가용성을 위해 설계된 스토리지 엔진이다.

### Memory

* 시스템 메모리를 사용하여 데이터를 저장하는 스토리지 엔진으로, 빠르지만 영구적이지 않다.

## 파일 시스템

데이터가 저장되는 기본 파일 시스템이다.

### 파일 & 로그

* 실제 데이터, 인덱스 정보, 복제를 위한 바이너리 로그, 트랜잭션을 위한 언두 로그, 성능 분석을 위한 일반 및 느린 쿼리 로그 등 다양한 유형의 데이터를 저장한다.
