# B+ tree

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>From Wiki</p></figcaption></figure>

### 균형 유지

* B+트리는 삽입과 삭제 시 항상 균형을 유지하는 구조를 가진다.
* 이는 검색,삽입,삭제 작업의 효율성을 보장 한다.

### 노드 구조

* 하나의 노드는 여러 개의 자식 노드를 가질 수 있으며, 이는 트리의 규형과 검색 효율성을 높인다.

### 리프 노드의 데이터

* 실제 데이터는 리프 노드에만 존재한다. 이는 연속적인 데이터 접근에 유리하며, 순차적인 읽기 작업에서 높은 성능을 제공한다.

### 노드 크기와 메모리 페이지

* B+트리의 노드 크기는 대개 데이터베이스 시스템의 메모리 페이지 크기에 맞춰 설정한다.

```bash
// Apple M1 Pro 가쥰
getconf PAGESIZE
16384 (16KB)
```

## Relation

[B+ tree WIKI](https://en.wikipedia.org/wiki/B%2B\_tree)&#x20;

[B+ tree 시뮬레이션](https://www.cs.usfca.edu/\~galles/visualization/BPlusTree.html)
