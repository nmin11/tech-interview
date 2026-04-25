## Kafka 개요

*Apache Kafka*

- LinkedIn에서 개발, 이후 Apache 오픈소스 프로젝트로 공개된 **분산 이벤트 스트리밍 플랫폼**
- 대용량, 고처리량, 실시간 데이터 스트리밍에 최적화
- **Pub/Sub 메시징 모델** 기반
- 데이터를 메모리가 아닌 **디스크에 순차 기록**해서 높은 내구성과 처리량을 동시에 달성

**핵심 특징**

- **고처리량**: 디스크 순차 I/O와 Zero-Copy 기술로 수백만 건/초 처리 가능
- **내구성**: 메시지를 디스크에 저장하고 복제(Replication)하여 장애 시에도 데이터 유실 방지
- **확장성**: Partition을 늘려서 수평 확장 가능
- **재처리 가능**: Offset 기반으로 과거 메시지를 재소비 가능

## 핵심 구성요소

### Broker

- Kafka 서버 단위
- 메시지를 저장하고 Producer/Consumer 요청을 처리
- 여러 Broker를 묶어 **Kafka Cluster**를 구성
- 각 Broker는 고유한 ID를 가짐

### Topic

- 메시지를 구분하는 **논리적 채널**
- 데이터베이스의 테이블과 유사한 개념
- 1개 이상의 Partition으로 구성

### Partition

- Topic을 구성하는 **물리적 저장 단위**
- 메시지가 순서대로 append-only로 기록됨
- Partition 단위로 병렬 처리 → 처리량 향상

```
Topic: "orders"
┌─────────────────────────────────────────────┐
│  Partition 0: [msg0] [msg1] [msg2] [msg3]   │
│  Partition 1: [msg0] [msg1] [msg2]          │
│  Partition 2: [msg0] [msg1] [msg3] [msg4]   │
└─────────────────────────────────────────────┘
```

### Offset

- Partition 내 각 메시지의 **고유한 순번**
- 0부터 시작하며 단조 증가
- Consumer는 자신이 어디까지 읽었는지(Committed Offset)를 관리

### Producer

- Kafka에 메시지를 **발행(Publish)** 하는 주체
- 어떤 Topic의 어떤 Partition으로 보낼지 결정 (Key 기반 or 라운드로빈)

### Consumer

- Kafka로부터 메시지를 **구독(Subscribe)** 하는 주체
- 각 Partition에서 메시지를 순서대로 읽음
- Consumer는 반드시 하나의 **Consumer Group**에 속함

### Consumer Group

- 동일한 Topic을 소비하는 Consumer들의 **논리적 그룹**
- 같은 Consumer Group 내에서 각 Partition은 **정확히 1개의 Consumer에게만 할당**
- 서로 다른 Consumer Group은 동일한 Partition을 **독립적으로** 소비 가능

### Zookeeper / KRaft

- **Zookeeper**: 기존에 Kafka 클러스터의 메타데이터(브로커 목록, 리더 정보 등) 관리에 사용
- **KRaft**: Kafka 2.8부터 도입된 자체 합의 프로토콜, Zookeeper 의존성 제거 (Kafka 3.x에서 권장)

## 고성능의 이유

- **순차 디스크 I/O**: 랜덤 접근이 아닌 append-only 순차 쓰기로 HDD에서도 높은 쓰기 성능
- **Zero-Copy**: 커널 버퍼에서 네트워크 소켓으로 데이터를 직접 전송 (CPU 개입 최소화)
- **Batch 처리**: Producer/Consumer 모두 메시지를 모아서 한 번에 처리
- **Page Cache 활용**: OS의 Page Cache를 적극 활용해서 읽기 성능 극대화

## Producer - Consumer 관계

```
                        Kafka Cluster
                   ┌──────────────────────┐
                   │  Topic: "orders"     │
Producer A ──────► │  Partition 0  ──────►│──────► Consumer 1
                   │                      │         (Group A)
Producer B ──────► │  Partition 1  ──────►│──────► Consumer 2
                   │                      │         (Group A)
                   │  Partition 2  ──────►│──────► Consumer 3
                   └──────────────────────┘         (Group A)
```

- Producer와 Consumer는 **직접 통신하지 않고** Broker(Topic/Partition)를 통해 분리됨
- 이 비동기 분리 구조 덕분에 Producer 속도와 Consumer 속도를 **독립적으로** 조정 가능

**Producer의 Partition 결정 방식**

- **Key 지정 시**: `hash(key) % partition_count` → 동일 Key는 항상 같은 Partition으로 전달되어 **순서 보장**
- **Key 없을 시**: 라운드로빈 또는 배치 단위 분산 → 전체 처리량 최대화

## Consumer Group 구독 관계

### Consumer 1개 + Partition 3개

- 1개의 Consumer가 모든 Partition을 혼자 담당

```
Topic: "orders"
┌───────────────────────────────────────────┐
│  Partition 0  ──────┐                     │
│  Partition 1  ──────┼──► Consumer 1       │
│  Partition 2  ──────┘     (Group A)       │
└───────────────────────────────────────────┘
```

- Consumer 1개가 모든 메시지를 순차 처리
- 병렬 처리 불가 → 처리량이 Partition 수에 비해 낮음
- Consumer가 죽으면 해당 Group의 소비가 전면 중단됨

### Consumer 수 = Partition 수 (이상적인 상태)

```
Topic: "orders"
┌───────────────────────────────────────────┐
│  Partition 0  ──────► Consumer 1          │
│  Partition 1  ──────► Consumer 2          │
│  Partition 2  ──────► Consumer 3          │
└───────────────────────────────────────────┘
                          (Group A)
```

- 각 Consumer가 전담 Partition을 소비 → 최적의 병렬 처리
- 한 Consumer 장애 발생 시 나머지 Consumer들이 해당 Partition을 재분배받음 (**Rebalancing**)

### Consumer 수 > Partition 수 (유휴 Consumer 발생)

```
Topic: "orders"
┌───────────────────────────────────────────┐
│  Partition 0  ──────► Consumer 1          │
│  Partition 1  ──────► Consumer 2          │
│                        Consumer 3  (유휴) │
└───────────────────────────────────────────┘
                          (Group A)
```

- Partition 수보다 많은 Consumer는 **아무 Partition도 할당받지 못하고 대기**
- Consumer 수를 늘려도 처리량이 증가하지 않음 → Partition 수가 병렬 처리 한계를 결정

### 여러 Consumer Group이 같은 Topic 구독

```
Topic: "orders"
┌──────────────────────────────────────────────────────────┐
│  Partition 0  ──────► Consumer 1 (Group A)               │
│                ──────► Consumer 1 (Group B)               │
│  Partition 1  ──────► Consumer 2 (Group A)               │
│                ──────► Consumer 2 (Group B)               │
└──────────────────────────────────────────────────────────┘
```

- 각 Consumer Group은 **독립적인 Offset**을 유지
- 같은 메시지를 여러 시스템이 각자의 목적으로 소비 가능 (결제 서비스, 알림 서비스, 분석 서비스 등)

## Rebalancing

- Consumer Group 내 Consumer 수가 변경될 때(추가/제거/장애) Partition 할당을 재조정하는 과정
- **Rebalancing 중에는 해당 Consumer Group의 소비가 일시 중단**되어 지연 발생 가능 (Stop-the-world)
- Kafka 2.4+에서 **Incremental Cooperative Rebalancing** 도입으로 영향 최소화

**Rebalancing 지연 대처 방법**

- `session.timeout.ms` / `heartbeat.interval.ms` 적절히 조정
- `max.poll.interval.ms` 내에 처리가 완료되도록 배치 크기 조정
- Kafka 2.4+ Incremental Cooperative Rebalancing 활용

## Replication

```
Topic: "orders", Partition 0, Replication Factor = 3

Broker 1: [Leader]   Partition 0 ◄── Producer Write
Broker 2: [Follower] Partition 0     (리더로부터 복제)
Broker 3: [Follower] Partition 0     (리더로부터 복제)
```

- 각 Partition은 1개의 **Leader**와 N-1개의 **Follower**로 구성
- **Producer/Consumer는 항상 Leader와 통신**
- Leader 장애 시 ISR(In-Sync Replica) 중 하나가 새 Leader로 선출
- **ISR**: Leader와 동기화 상태를 유지하고 있는 Replica 집합
- `min.insync.replicas` 설정으로 최소 ISR 수를 지정하면, 그 수만큼 복제가 완료된 경우에만 Producer ACK를 반환 → 데이터 유실 방지

## 순서 보장

- **Partition 내**에서는 메시지 순서가 보장됨
- Topic 전체의 순서를 보장하려면 Partition을 1개로 설정하거나, 관련 메시지에 동일한 Key를 부여하여 같은 Partition으로 라우팅

## 메시지 전달 보장 방식

| 방식                | 설명                               | 중복 가능성 | 유실 가능성 |
| :-----------------: | :--------------------------------: | :---------: | :---------: |
| **At Most Once**    | 메시지를 최대 1번 전달 (재전송 없음) | 없음        | 있음        |
| **At Least Once**   | 메시지를 최소 1번 전달 (재전송 가능) | 있음        | 없음        |
| **Exactly Once**    | 정확히 1번 전달 보장                | 없음        | 없음        |

**At Most Once**

- Producer: `acks=0` → Broker ACK를 기다리지 않고 바로 다음 메시지 전송
- Consumer: 메시지를 읽은 직후 Offset을 커밋하고 처리 → 처리 중 실패해도 재소비 안 함

**At Least Once** _(Kafka 기본 동작)_

- Producer: `acks=1` 또는 `acks=all` + `retries > 0` → ACK 미수신 시 재전송
- Consumer: 처리 완료 후 수동으로 Offset 커밋 (`enable.auto.commit=false`)

**Exactly Once**

- Producer: `enable.idempotence=true` + `acks=all` + `retries > 0` → 중복 전송 자동 제거
- Transactional API: `transactional.id` 설정으로 여러 Partition에 원자적 쓰기
- Consumer: `isolation.level=read_committed` → 커밋 완료된 메시지만 읽음

## Offset 관리

- Consumer는 메시지를 읽은 후 `__consumer_offsets` 토픽에 Committed Offset을 저장
- 장애 후 재시작 시 마지막 Committed Offset 이후부터 재소비

**Auto Commit vs Manual Commit**

- **Auto Commit** (`enable.auto.commit=true`): 일정 주기마다 자동 커밋 → 처리 전 커밋 가능성으로 유실 위험
- **Manual Commit**: 처리 완료 후 명시적 커밋 → At Least Once 보장에 적합

## Kafka vs 전통적인 메시지 큐

|                   | Kafka                           | RabbitMQ / ActiveMQ         |
| :---------------: | :-----------------------------: | :-------------------------: |
| **메시지 보존**   | 설정된 기간/크기까지 디스크 보존 | 소비 후 즉시 삭제            |
| **재처리**        | Offset 조정으로 재처리 가능      | 재처리 어려움               |
| **처리량**        | 매우 높음 (수백만 건/초)         | 상대적으로 낮음             |
| **소비 모델**     | Pull 방식                        | Push 방식                   |
| **파티셔닝**      | Partition 단위 병렬 처리         | Queue 단위                  |
| **적합한 사용처** | 로그 수집, 이벤트 스트리밍, CDC  | 작업 큐, RPC, 복잡한 라우팅 |

Reference

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
