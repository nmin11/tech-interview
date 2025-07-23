## Index란?

추가 저장 공간을 활용해서 테이블에 대한 검색 속도를 향상시키는 자료구조다.  
인덱스를 사용하지 않는 경우 조회를 위해 데이터 Full Scan을 하기 때문에 인덱스를 활용해서 조회 속도를 향상시킬 수 있다.  
하지만 인덱스를 관리하기 위해 DB의 저장공간을 할애해야 하며, 수정 작업이 빈번한 속성에 인덱스를 걸면 인덱스의 크기가 커져서 오히려 성능이 저하될 수 있다.  
따라서 인덱스는 조회 위주의 필드, JOIN, WHERE, ORDER BY 조건으로 사용되는 컬럼에 사용해주는 것이 적절하다.  
그리고 인덱스는 효율적인 레코드 접근을 위해 순서 매김 알고리즘을 제공한다.  
알고리즘의 종류에는 B-tree Index, Bitmap Index, IOT Index, Clustered Index 등이 있으며, 주로 사용되는 것은 B-tree 구조다.

Reference

- [DB 인덱싱이란?](https://velog.io/@bsjp400/Database-DB-%EC%9D%B8%EB%8D%B1%EC%8B%B1Indexing%EC%9D%B4%EB%9E%80)
- [인덱스란?](https://mangkyu.tistory.com/96)
