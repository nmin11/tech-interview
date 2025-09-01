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

## CDC

*Change Data Capture*

**개념**

- DB 변경사항(INSERT/UPDATE/DELETE)을 실시간으로 캡쳐해서 저장해두는 방식
- 데이터 실시간 동기화, 감사 및 분석이 필수적인 경우 핵심적인 컴포넌트
- 분산 시스템 간 데이터 일관성을 보장

**특장점**

- 데이터 실시간 동기화
  - 연결된 모든 분산 시스템의 실시간 업데이트 보장
- EDA(Event-Driven Architecture)
  - 데이터 변경사항을 이벤트 기반으로 캡쳐
  - 분산 시스템들이 이벤트에 따라 동적으로, 실시간으로 반응하도록 해줌
- 데이터 처리 파이프라인
  - 데이터 변경사항을 지속적으로 스트리밍하는 구조이기 때문에 수동 작업 혹은 배치 처리 작업 최소화
- 확장성 및 유연성
  - 컴포넌트를 분리하고 비동기 방식을 활용하므로 시스템의 수평적 확장을 지원하면서, 빠른 응답과 안정성 유지
- 분석 및 인사이트
  - 최신 데이터로부터 인사이트 도출 가능
  - 분석 플랫폼과의 통합 가능

**구현 방식**

- Log-based
  - DB 트랜잭션 로그 혹은 복제 로그를 활용한 캡쳐 방식
  - DB 로그를 모니터링 및 분석해서 변경 이벤트를 추출한 후에 전파하는 과정 포함
  - 낮은 지연, 높은 정확성을 보장하여 실시간 데이터 동기화에 적합
- Trigger-based
  - DB 테이블에 트리거를 추가해서 캡쳐하는 방식
  - 테이블에 INSERT/UPDATE/DELETE 작업 발생 시 이벤트 기록을 위한 커스텀 로직 실행
  - DB 로그에 접근할 수 없거나 신뢰할 수 없는 경우에 사용하는 방식
- Pub/Sub
  - 원본 시스템이 변경 사항을 '퍼블리싱' 하고 대상 시스템이 이를 '구독' 하는 방식
  - 퍼블리셔는 데이터 변경사항을 캡쳐 후 메시지 브로커 혹은 이벤트 버스에 발행하고, 구독자는 이벤트를 받아서 DB 혹은 시스템에 반영
  - 분산 시스템에서 결합도를 낮추고 확장성 및 유연성을 높이기 위한 방식
- Change Data Mesh
  - 변경 이벤트 캡쳐, 처리 및 사용 책임을 탈중앙화해서 개별 서비스나 도메인에 분산시키는 방식
  - 각 서비스나 도메인은 변경 데이터를 자체적으로 관리해야 하며, 더 큰 자율성 및 확장성이 생김
  - 탈중앙화된 분산형 이벤트 중심 아키텍처에 적합

Reference

- [GeeksforGeeks - CDC](https://www.geeksforgeeks.org/system-design/change-data-capture-cdc/)

## Inbox / OutBox 패턴

### Inbox 패턴

- 동일 메시지가 여러 번 수신되어도 한번만 처리하는 기법
- 수신된 메시지를 Inbox 테이블에 저장해둠으로써 중복 여부를 확인

### Outbox 패턴

- 메시지를 전달한 쪽에서 메시지가 정상적으로 전달되었음을 보장
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

## MDC

*Mapped Diagnostic Context*

**개념**

- Java 진영에서 Logback 프레임워크를 통해 사용할 수 있는 기능
  - Log4j, Log4j2, SLF4J 에서 사용 가능
- 로그 메시지에 컨텍스트 정보를 담을 수 있게 해줌
- Appender를 통해 액세스할 수 있는 정보들을 Map과 같은 구조로 채울 수 있음
- 내부적으로 `ThreadLocal`을 활용해서 컨텍스트 정보를 저장하는 방식

**장점**

- 모든 요청의 시작부터 종료까지의 컨텍스트 정보를 추적할 수 있음
- 스레드 고유 식별자 역할을 수행하는 `X-Correlation-Id`를 통해, 분산 시스템에서 end-to-end 추적을 가능하게 함

Reference

- [Robin Chen - Enhancing Logging in Spring Boot with MDC](https://medium.com/@sudacgb/enhancing-logging-in-spring-boot-with-mapped-diagnostic-context-mdc-a-step-by-step-tutorial-0a57b0304dd3)
- [Baeldung - Improved Java Logging with MDC](https://www.baeldung.com/mdc-in-log4j-2-logback)
