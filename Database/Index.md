## Index

**정의**

- 테이블의 특정 컬럼에 대한 쿼리 속도를 향상시키는 자료구조
- 도서의 '색인'처럼 값이 저장된 위치(레코드 주소)를 빠르게 찾아줌
- UNIQUE, PK, FK 제약 조건에도 사용됨

**장점**

- SELECT, JOIN, WHERE, ORDER BY 연산에서 탐색 속도 향상 (디스크 I/O 최소화)
- 일반 조희 시의 테이블 Full Scan을 피할 수 있음

**단점**

- 인덱스 대상에 대한 쓰기 작업 시 성능 저하
- 인덱스 자체가 별도의 저장 공간을 차지

**인덱싱 방식**

- clustered
  - 테이블 전체가 인덱스 컬럼에 대해 정렬
  - 테이블 1개 당 1개만 가능
  - 생성, 수정, 삭제 시 데이터 재정렬 작업 필요
- non-clustered
  - 테이블과 매핑된 별도의 인덱스 객체를 생성해서 관리 (인덱스 컬럼을 정렬)
  - 여러 개의 인덱싱 가능
  - 데이터 조회 시 RID LookUp 과정 필요
  - clustered 방식에 비해 더 많은 공간 필요

- 단일 컬럼 / 복합 인덱스

**인덱스 알고리즘**

- B-Tree / B+Tree: 균형 이진 트리 기반, 범위 검색에 강점, **범용적**
- Bitmap: 비트맵(0/1) 배열 활용, 논리 연산(OR/AND) 빠름, **낮은 카디널리티에 적합**
- Hash: 해시 함수 기반, **단일값 조회 시 O(1)**
- etc
  - GiST, GIN: text, gee, json 등 **복합적인 비정형 데이터** 특화
  - Fractal-Tree, BRIN: 로그, 벌크 삽입 등 **쓰기 집중 환경** 특화

Reference

- [Wikipidea - Database index](https://en.wikipedia.org/wiki/Database_index)
- [bsjp400 - DB 인덱싱이란?](https://velog.io/@bsjp400/Database-DB-%EC%9D%B8%EB%8D%B1%EC%8B%B1Indexing%EC%9D%B4%EB%9E%80)
- [망나니개발자 - 인덱스란?](https://mangkyu.tistory.com/96)

## Covering Index

**정의**

- 쿼리에 필요한 모든 컬럼이 인덱스에 포함되어, 테이블에 접근하지 않고 인덱스만으로 쿼리를 처리할 수 있게 하는 것

**장점**

- 디스크 I/O 최소화
- 쿼리 응답 속도 향상

**단점**

- 인덱스 크기 증가로 인한 저장 공간 증가
- 쓰기 작업 시 인덱스 갱신에 대한 오버헤드 증가

**적합 케이스**

- 자주 조회되는 쿼리
- `SELECT` 컬럼 수가 적은 경우
- 읽기 위주의 테이블

## 복합 인덱스 (Composite Index)

**정의**

- 두 개 이상의 컬럼을 조합하여 생성하는 인덱스
- 컬럼의 순서가 인덱스 활용에 큰 영향을 미침

**Leftmost Prefix Rule**

- 복합 인덱스는 정의된 컬럼 순서대로 왼쪽부터 적용됨
- 중간 컬럼을 건너뛰면 그 이후 컬럼은 인덱스를 활용하지 못함

```sql
-- 인덱스: (A, B, C)

WHERE A = 1                    -- O 인덱스 사용
WHERE A = 1 AND B = 2          -- O 인덱스 사용
WHERE A = 1 AND B = 2 AND C = 3 -- O 인덱스 사용 (전체)
WHERE B = 2                    -- X 인덱스 미사용 (A가 없음)
WHERE A = 1 AND C = 3          -- △ A만 인덱스 사용 (B를 건너뜀)
```

**범위 조건의 영향**

- 범위 조건(`>`, `<`, `BETWEEN`, `LIKE`)이 사용되면 그 이후 컬럼은 인덱스를 효율적으로 활용하지 못함

```sql
-- 인덱스: (A, B, C)

WHERE A = 1 AND B > 5 AND C = 3
-- A, B까지 인덱스 사용, C는 필터링만 수행
```

**컬럼 순서 결정 기준**

1. **동등 조건(=)을 범위 조건보다 앞에**
   - `=` 조건이 있는 컬럼을 앞에 배치해야 뒤 컬럼까지 인덱스 활용 가능

2. **카디널리티가 높은 컬럼을 앞에**
   - 카디널리티: 컬럼이 가질 수 있는 고유 값의 수
   - 높은 카디널리티 = 더 빠른 필터링

3. **자주 사용되는 쿼리 패턴 고려**
   - WHERE 절에 자주 등장하는 컬럼 조합을 분석

Reference

- [MySQL 공식문서 - Multiple-Column Indexes](https://dev.mysql.com/doc/refman/8.0/en/multiple-column-indexes.html)
- [USE THE INDEX, LUKE - Concatenated Indexes](https://use-the-index-luke.com/sql/where-clause/the-equals-operator/concatenated-keys)
