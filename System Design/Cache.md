## 캐싱의 종류들

### 브라우저 캐싱

- 사용자 PC에 웹페이지 리소스 저장
- 사용자가 다음에 다시 방문했을 때 로컬 리소스로부터 페이지를 로드할 수 있게 해줌
- 서버 로드 과정을 없애주기 때문에 페이지 로딩 시간이 빨라짐

### CDN 캐싱

- 데이터를 다양한 지리적 위치에 복제해두고, 사용자에게 가장 가까운 서버에서 데이터를 서빙하게 하는 방식
- AWS 등의 클라우드 서비스에서 CDN 서비스를 제공해줌
- 글로벌 사용자가 많을수록 빛을 발함

### DB 쿼리 캐싱

- DBMS에서 동일한 쿼리에 대한 결과를 캐싱
- `MySQL Query Cache` 같은 DB 특화 솔루션, 혹은 `Hibernate` 같은 ORM을 통해 활용 가능

### In-Memory 캐싱

- 디스크 공간보다 빠른 메모리 영역에 캐싱하는 방식
- 자주 조회되는 데이터, 세션 정보, 풀 페이지 조회에 주로 사용됨

### 애플리케이션 캐싱

- 애플리케이션에서 생성 비용이 많이 드는 인스턴스나 데이터를 캐싱하는 방식
- 애플리케이션에서 직접 커스텀하거나, `Spring Cache` 같은 프레임워크 제공 방식을 활용할 수 있음

### 분산 캐싱

- 다중 서버 간 공유되는 캐시
- 시스템의 모든 노드에 캐시를 복제해 성능과 확장성을 높이는 방식
- Redis, Memcached 등이 많이 활용됨

Reference

- [Abhishek Ranjan - Caching in System Design: An In-Depth Exploration](https://medium.com/%40abhishekranjandev/caching-in-system-design-an-in-depth-exploration-b51e2c2e4dbd)
