## 2PC

*two-phase commit*

**분산 시스템의 모든 노드가 모두 커밋되거나 모두 롤백되는 것을 원자적으로 보장하는 프로토콜**

```
Coordinator                                         Cohort
                              QUERY TO COMMIT
                -------------------------------->
                              VOTE YES/NO           prepare*/abort*
                <-------------------------------
commit*/abort*                COMMIT/ROLLBACK
                -------------------------------->
                              ACKNOWLEDGMENT        commit*/abort*
                <--------------------------------  
end
```

### 역할

- Coordinator: 트랜잭션 관리자 노드
- Cohort: Coordinator 요청을 받고 트랜잭션 작업 준비 상태를 YES/NO 로 알림

### 작업 단계

- 준비 단계
  - 코디네이터는 모든 참여자 노드에게 준비 요청을 전송
  - 참여자 노드들은 현 상태 및 자원들을 가지고 트랜잭션을 처리할 수 있는지 체크
  - 참여자 노드들은 체크 결과에 따라 YES 또는 NO 응답
- 커밋 단계
  - 모든 참여자 노드가 YES를 준 경우 트랜잭션 커밋, 하나라도 NO인 경우 트랜잭션 롤백
  - 자원들에 대한 락이 걸려있었다면 해제하고 작업 수행, 이후 코디네이터에게 작업 완료를 알림

### Write-Ahead Log

- 핵심: 변경사항을 실제 데이터 적용에 앞서 로그로 먼저 기록해둔다
- 2PC의 참여자 노드들의 작업에 대해 원자성 및 지속성을 보장하도록 해줌

Reference

- [Wikipedia - 2단계 커밋 프로토콜](https://ko.wikipedia.org/wiki/2단계_커밋_프로토콜)
- [Humanscape Jake - ACID와 2PC](https://medium.com/humanscape-tech/acid%EC%99%80-2pc-30bef7f59331)
- [Baeldung - Difference Between Two-Phase Commit and Saga Pattern](https://www.baeldung.com/cs/two-phase-commit-vs-saga-pattern)

## Inbox / OutBox 패턴

### Inbox 패턴

- 동일 메시지가 여러 번 수신되어도 한번만 처리하는 기법
- 수신된 메시지를 Inbox 테이블에 저장해둠으로써 중복 여부를 확인

### Outbox 패턴

- 비즈니스 로직 처리 + 이벤트 발행을 원자적으로 실행하는 방식
  - 단일 트랜잭션 내에서 함께 실행
- 데이터의 결과적 일관성 보장, 실패한 로직의 재시도 가능

## Transactional Outbox 패턴

**개념**

- 분산 시스템에서 DB 업데이트와 메시지 발행을 원자적으로 처리하는 디자인 패턴
- 2개의 쓰기 작업을 동시에 할 때 한쪽 작업만 성공할 수 있다는 **Dual Write** 문제를 해결할 수 있게 해줌
- 비즈니스 로직 수행과 아웃박스 테이블 저장 로직이 단일 트랜잭션 내에서 수행되어야 한다는 것이 핵심!
- Spring에서는 `BEFORE_COMMIT`으로 아웃박스 테이블 기록 작업을 하고, `AFTER_COMMIT`으로 메시지 전송 작업을 하는 식으로 구현할 수 있음

**장점**

- 복잡한 2PC 없이도 데이터 일관성을 보장할 수 있음
- 아웃박스 테이블을 활용해서 실패한 메시지를 재전송할 수 있으므로 내결함성 보장
- at least once 배송 보장

Reference

- [29CM - 트랜잭셔널 아웃박스 패턴의 실제 구현 사례](https://medium.com/@greg.shiny82/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%94%EB%84%90-%EC%95%84%EC%9B%83%EB%B0%95%EC%8A%A4-%ED%8C%A8%ED%84%B4%EC%9D%98-%EC%8B%A4%EC%A0%9C-%EA%B5%AC%ED%98%84-%EC%82%AC%EB%A1%80-29cm-0f822fc23edb)
