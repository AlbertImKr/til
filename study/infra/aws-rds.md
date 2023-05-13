---
description: Relational Database Service
---

# AWS RDS

## MariaDB 장점

1. RDS의 가격은 라이센스 비용의 여향을 받는다. 상용 데이터베이스인 오라클,MSSQL이 오픈소스인 MySQL,MariaDB,PostgreSQL 보다는 동일한 사양 대비 더 가격이 높습니다.
2. Amazon Aurora 교체 용이성입니다.
   * Amazon Aurora는 AWS에서 MySQL과 PostgreSQL을 클라우드 기반에 맞게 재구성한 데이터베이스 입니다.&#x20;
   * 공식 자료에 의하면 RDS MySQL 대비 5배, RDS PostgreSQL 보다 3배의 성능을 제공합니다.
   * 더군다나 AWS에서 직접 엔지니어링하고 있기 때문에 계속해서 발전하고 있습니다.
3. 단순 쿼리 처리 성능이 어떤 제품보다 압도적이며 이미 오래 사용되어 왔기 때문에 성능과 신뢰성 등에서 꾸준히 개선되어 왔다.
   * [MySQL에서 MariaDB로 마이그레이션 해야 할 10가지 이유](https://www.leafcats.com/12)

## RDS 운영환경에 맞는 파라미터 설정

* 타임존
  * time\_zone
* Character Set
  * character -> 8개  -> utf8mb4(utf8 + 이모지 저장기능)
  * collation\_connection -> utf8mb4\_general\_ci
  * collation\_server -> utf8mb4\_general\_ci
* Max Connection
  * 인스턴스 사양에 따라 자동으로 정한다.

## Refrence

> :books:[스프링 부트와 AWS로 혼자 구현하는 웹 서비스](https://product.kyobobook.co.kr/detail/S000001019679)
