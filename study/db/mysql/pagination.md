# Pagination

## 페이지네이션

* 사용자에게 데이터를 페이지 형태로 나누어 제공하는 방법이다.
* 대용량 데이터를 효과적으로 관리하고,사용자 인터페이스에 데이터를 더욱 적절히 표시하는 데 도움이 된다.

#### Size

* 한 페이지에 표시되는 데이터의 수
* 예를 들어, 웹 페이지에서 게시판의 한 페이지에 10개의 글을 보여줄 경우, 'size'는 10이다.

#### Page

* 사용자가 요청하는 특정 페이지 번호
* 예를 들어, 사용자가 세 번째 페이지를 요청할 경우, 'page'는 3이 된다.

#### Sort

* 데이터 정렬 순서
* 예를 들어, 최신 글을 먼저 보여주기 위해 '작성일자' 컬럼을 기준으로 내림차순 정렬을 할 수 있다.

## 최적화

### 인덱스 활용

* 페이지네이션에 사용되는 컬럼(예: 게시물의 '작성일자'나 'ID')에 인덱스를 적용하여 검색 속도를 향상시킨다.

### 커서 기반 페이지네이션

* 오프셋과 리밋을 사용하는 대신 커서 기반 페이지네이션을 활용한다.
* 예를 들어, 마지막으로 조회한 게시물의 ID를 기준으로 다음 데이터를 조회하는 방식이다.

### 미리 계산된 페이지네이션

* 사용자가 자주 접근하는 페이지 정보(예: 인기 게시물 목록)를 미리 계산하여 저장해두고, 요청시 빠르게 제공할 수 있다.

### 캐싱 사용

* 자주 요청되는 페이지 데이터(예: 홈페이지의 첫 페이지)를 캐시하여 빠르게 응답할 수 있도록 한다.

## 커버링 인덱스

* 커버링 인덱스는 쿼리의 결과를 만족시키는 모든 데이터가 인덱스에 포함되어 있는 경우를 말한다

### 검색 조건 일치

* 쿼리에서 요구하는 모든 필드가 인덱스에 포함되어 있어야 한다.
* 예를 들어, '사용자명'과 '가입일'을 기준으로 사용자를 조회하는 쿼리가 있을 때, 이 두 컬럼 모두를 포함하는 인덱스를 만들면 커버링 인덱스가 된다.

### 테이블 접근 최소화

* 커버링 인덱스를 사용하면 실제 테이블에 접근할 필요 없이 인덱스에서 모든 필요한 데이터를 가져올 수 있다.

### 성능 향상

* 데이터베이스는 인덱스에서 바로 결과를 가져올 수 있으므로, 디스크 I/O가 감소하고 전체적인 쿼리 성능이 향상한다.