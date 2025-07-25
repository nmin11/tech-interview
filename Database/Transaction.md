## Transaction이란?

여러 개의 쿼리를 논리상 하나의 작업 단위로 묶어주는 것을 뜻한다.  
데이터베이스에서 트랜잭션은 작업 단위가 모두 성공했을 때 `Commit`이 되거나, 하나라도 실패했을 때 `Rollback`이 되는 것을 보장한다.

그리고 ACID라고 불리는 특징들을 통해 이점을 제공한다.

- Atomicity(원자성): 트랜잭션은 모두 DB에 반영되거나, 혹은 전혀 반영되지 않아야 한다.
- Consistency(일관성): 트랜잭션 작업 처리 결과는 항상 일관성이 있어야 한다.
- Isolation(독립성): 둘 이상의 트랜잭션이 동시에 실행되고 있다면 서로 다른 트랜잭션의 연산에 끼어들 수 없다.
- Durability(지속성): 트랜잭션이 성공적으로 완수되었다면 결과는 영구히 반영되어야 한다.

## 트랜잭션 격리 수준

**정의**

- 동시에 수행되는 트랜잭션들이 서로의 데이터 변경사항에 어떻게 영향을 받는지를 제어하는 방식
- 트랜잭션의 ACID 특성 중 **Isolation**에 해당하는 부분

**격리 수준에 따라 방지 가능한 이상 현상들**

- **Dirty Read** : uncommit 된 트랜잭션의 데이터를 읽는 현상
- **Non-repeatable Read** : 이전에 읽었던 데이터가 다른 트랜잭션의 commit에 의해 변경되는 현상
- **Phantom Read** : 쿼리 범위의 데이터들이 다른 트랜잭션의 commit에 의해 변경되는 현상

**ANSI SQL 표준 격리 수준**

| 격리 수준              | Dirty Read | Non-repeatable Read | Phantom Read |
|:-----------------------:|:----------:|:-----------:|:-----------:|
| READ UNCOMMITTED        | ✅         | ✅          | ✅          |
| READ COMMITTED          | ❌         | ✅          | ✅          |
| REPEATABLE READ         | ❌         | ❌          | ✅          |
| SERIALIZABLE            | ❌         | ❌          | ❌          |

- **READ UNCOMMITTED**
  - uncommit 데이터도 조회 가능
  - RDBMS 표준에서도 인정하지 않는 비권장 방식
- **READ COMMITTED**
  - commit 된 데이터만 조회 가능
  - 반복 읽기 수행 시 다른 트랜잭션의 commit 여부에 따라 조회 여부가 달라질 수 있음
- **REPEATABLE READ**
  - 변경 전 데이터를 백업해서, 변경 전/후 데이터를 모두 유지
  - MVCC(Multi-Version Concurrency Control, 다중 버전 동시성 제어)라고 부름
    - 롤백 시의 데이터 복원 뿐만 아니라, 서로 다른 트랜잭션 간 접근 데이터를 세밀하게 제어
  - 한 트랜잭션 내에서의 동일한 결과 보장
  - `SELECT` 이후 `SELECT FOR UPDATE` 활용 시 Phantom Read 발생 가능
    - MVCC의 undo log가 아닌, 실제 테이블을 조회하게 되기 때문
- **SERIALIZABLE**
  - 가장 엄격한 격리 수준
  - 트랜잭션은 순차적으로 처리되며, 여러 트랜잭션이 동일한 레코드에 동시 접근할 수 없음
  - 동시 처리 성능은 매우 떨어짐
  - `SELECT FOR SHARE/UPDATE`가 아닌 순수 `SELECT`에서도 공유락(Shared Lock) 활용

Reference

- [IBM - Isolation level](https://www.ibm.com/docs/en/netezza?topic=tc-isolation-level)
- [망나니개발자 - 트랜잭션의 격리 수준(Isolation Level)에 대해 쉽고 완벽하게 이해하기](https://mangkyu.tistory.com/299)
