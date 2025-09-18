## @Transactional

### 동작 원리

- Spring은 `TransactionManager` / `PlatformTransactionManager` 인터페이스를 제공
  - 구현체: `DataSourceTransactionManager` `JpaTransactionManager` 등
- TransactionManager가 선정되면 `ProxyConfiguration` 클래스를 통해 트랜잭션 관리 기능 활성화
- CGLib, JDK Dynamic Proxy 등의 방식을 활용해 프록시 생성
- Spring AOP는 프록시를 활용해 메서드 호출을 가로채서 트랜잭션 관련 추가 동작 수행

### Propagation 설정

- `REQUIRED`(default): 트랜잭션이 필요하므로, 없으면 새로 만들고 있으면 기존 트랜잭션 사용
- `REQUIRES_NEW`: 항상 새 트랜잭션을 만듦
- `NESTED`: 기존 트랜잭션과 중첩된 트랜잭션 생성, 기존 트랜잭션이 없다면 새 트랜잭션을 생성해서 사용
- `SUPPORTS`: 트랜잭션이 있다면 지원해주고, 없다면 그냥 트랜잭션 없이 실행
- `NOT_SUPPORTED`: 트랜잭션이 있어도 중단하며, 트랜잭션을 지원하지 않음
- `MANDATORY`: 반드시 트랜잭션이 존재해야 하며, 없으면 예외 발생
- `NEVER`: 반드시 트랜잭션이 없어야 하며, 있으면 예외 발생

### Isolation 설정

- `READ_UNCOMMITTED`: 커밋되지 않은 데이터까지 조회 (dirty read)
- `READ_COMMITTED`: 커밋된 데이터만 조회 (non-repeatable read)
- `REPEATABLE_READ`: 스냅샷을 활용한 조회 (phantom read)
- `SERIALIZABLE`: 동시 작업이 안되는 조회

Reference

- [양일표 - @Transactional 바르게 알고 사용하기](https://medium.com/gdgsongdo/transactional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-7b0105eb5ed6)
- [Marco Behler - @Transactional In-Depth](https://www.marcobehler.com/guides/spring-transaction-management-transactional-in-depth)
