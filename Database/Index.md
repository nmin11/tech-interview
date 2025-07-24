## Index란?

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
