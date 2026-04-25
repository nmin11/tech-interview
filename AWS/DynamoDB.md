## DynamoDB LSI vs GSI

_Local Secondary Index & Global Secondary Index_

### Key Difference

|     구분      |           LSI            |          GSI           |
| :-----------: | :----------------------: | :--------------------: |
|   파티션 키   |    기본 테이블과 동일    |          자유          |
|    정렬 키    | 기본 테이블과 달라야 함  |          자유          |
|   생성 시점   | 테이블 생성 시 함께 생성 |          자유          |
|   크기 제한   |      파티션 당 10GB      |         무제한         |
|  처리량 공유  |    기본 테이블과 공유    |     독립적 처리량      |
|    일관성     |    Strong Consistency    | Eventually Consistency |
| 업데이트 방식 |          동기식          |        비동기식        |

### LSI

- 기본 테이블과 동일한 파티션 키, 다른 정렬 키를 가지는 인덱스
- 같은 파티션 키를 가진 항목들에 대한 추가 쿼리 지원
- 기본 테이블과 같은 파티션을 공유하므로 LSI에 대한 업데이트는 기본 테이블과 함께 원자적으로 처리됨
- 예시: 특정 사용자의 게시글을 날짜순으로 정렬할 때
- 필요한 경우들
  - Strong Consistency를 보장해야 할 때
  - 같은 파티션 키, 다른 정렬 순서가 필요할 때
  - 읽기 작업 시 Read Through가 필요한 경우

### GSI

- 기본 테이블과 다른 파티션 키, 다른 정렬 키를 가질 수 있는 인덱스
- 테이블 전체에 걸쳐서 쿼리 지원
- 비동기적으로 업데이트되므로, 결과적 일관성을 보장하는 방식
- 예시: 이메일 주소로 모든 사용자를 검색할 때
- 필요한 경우들
  - 기본 테이블 파티션 키와는 다른 속성으로 쿼리해야 할 때
  - 10GB 이상의 대용량 데이터를 인덱싱할 때
  - 차후 인덱스 추가/삭제가 필요한 경우

## DynamoDB Scan vs Query

### Key Difference

|      구분      |              Scan              |              Query              |
| :------------: | :----------------------------: | :-----------------------------: |
|   검색 범위    |          테이블 전체           |     특정 파티션 키의 항목들     |
|      성능      |        느림 (전체 스캔)        |       빠름 (인덱스 활용)        |
|      비용      |     높음 (모든 항목 읽기)      |      낮음 (필요한 항목만)       |
|     필터링     |   읽은 후 필터링 (비효율적)    |   파티션 키로 필터링 (효율적)   |
| 파티션 키 필요 |            선택사항            |              필수               |
|    정렬 키     |           사용 불가            |         조건 검색 가능          |
|  처리량 소비   | 읽은 모든 항목에 대해 RCU 소비 | 반환된 항목에 대해서만 RCU 소비 |
|  페이지네이션  |              지원              |              지원               |

### Scan

**개념**

- 테이블의 모든 항목을 순차적으로 읽는 작업
- 필터 표현식을 넣더라도 먼저 모든 항목을 조회한 후 필터링이 적용되는 방식

**특징**

- 테이블 전체를 스캔하므로 데이터가 많을수록 느려짐
- 모든 데이터에 대해 비용 발생
- 병렬 스캔을 통해 처리 속도 향상 가능
- 1MB씩 데이터를 읽고, 필요시 LastEvaluatedKey로 페이지네이션

**사용 케이스**

- 테이블의 모든 데이터를 분석해야 할 때
- 데이터 마이그레이션 또는 백업
- 작은 테이블에서 모든 항목 조회
- 파티션 키를 모르는 상태에서 전체 검색이 필요한 경우

**주의사항**

- 프로덕션 환경에서는 가급적 사용 지양 (성능 및 비용)
- 대용량 테이블에서는 처리량 제한으로 인한 throttling 발생 가능
- 필터 표현식은 읽기 후 적용되므로 RCU 절약 불가

**예시**

```javascript
// 모든 항목 스캔
const params = {
  TableName: "Users",
  FilterExpression: "age > :age", // 읽은 후 필터링
  ExpressionAttributeValues: {
    ":age": 25,
  },
};

const result = await dynamodb.scan(params).promise();
```

### Query

**개념**

- 파티션 키를 기반으로 특정 항목들을 효율적으로 조회하는 작업
- 세부적인 조건으로 검색 가능

**특징**

- 파티션 키 값을 반드시 지정해야 함
- 정렬 키로 범위 조건 검색 가능 (`=`, `<`, `>`, `BETWEEN`, `begins_with` 등)
- 인덱스를 활용하여 빠른 검색
- 반환된 항목에 대해서만 RCU 소비
- 특정 파티션 내에서만 작동하므로 효율적

**사용 케이스**

- 특정 사용자의 모든 주문 조회 (파티션 키: userId)
- 특정 기간의 로그 조회 (파티션 키: date, 정렬 키: timestamp)
- 특정 카테고리의 상품 목록 (파티션 키: categoryId)
- GSI/LSI를 활용한 대체 키 검색

**장점**

- Scan보다 훨씬 빠르고 비용 효율적
- 필요한 데이터만 읽으므로 처리량 효율적
- 인덱스 활용으로 확장성 우수

**예시**

```javascript
// 특정 파티션 키로 쿼리
const params = {
  TableName: "Users",
  KeyConditionExpression: "userId = :userId AND createdAt > :date",
  ExpressionAttributeValues: {
    ":userId": "user123",
    ":date": "2024-01-01",
  },
};

const result = await dynamodb.query(params).promise();
```

## DynamoDB 스키마 설계

### 스키마리스(Schema-less)의 의미

- DynamoDB는 테이블 생성 시 **파티션 키(+ 선택적 정렬 키)만 고정**하고, 나머지 Attribute는 항목마다 자유롭게 다를 수 있음
- 필드를 미리 선언하지 않아도 되지만, 그렇다고 설계 없이 아무렇게나 써도 된다는 의미는 아님
- **Access Pattern을 먼저 정의하고, 거기에 맞춰 키를 설계**하는 것이 핵심

### Single Table Design vs Multi Table Design

| 구분 | Single Table Design | Multi Table Design |
| :-: | :-: | :-: |
| **테이블 수** | 1개에 여러 엔티티 혼재 | 엔티티별로 테이블 분리 |
| **조회 효율** | 관련 엔티티를 1번의 Query로 조회 가능 | Join 불가 → 여러 번 조회 필요 |
| **복잡도** | 키 설계가 복잡 | 직관적 |
| **비용** | 낮음 (요청 수 최소화) | 높아질 수 있음 |
| **적합한 경우** | 엔티티 간 관계가 많고 함께 조회하는 경우 | 엔티티가 독립적이고 단순한 경우 |

### Single Table Design 키 설계 패턴

- 파티션 키와 정렬 키를 범용적으로 `pk`, `sk`처럼 추상화하여 여러 엔티티를 한 테이블에 수용

```
pk               sk                  기타 Attribute
─────────────────────────────────────────────────────────────
USER#user123     METADATA            name, email, ...
USER#user123     VIDEO#vid001        status, createdAt, ...
USER#user123     VIDEO#vid002        status, createdAt, ...
VIDEO#vid001     TRANSCODE#job001    inputUrl, outputUrl, ...
VIDEO#vid001     TRANSCODE#job002    inputUrl, outputUrl, ...
```

- `pk=USER#user123` + `sk begins_with VIDEO#` → 특정 사용자의 영상 목록 한 번에 조회
- `pk=VIDEO#vid001` + `sk begins_with TRANSCODE#` → 특정 영상의 트랜스코딩 이력 한 번에 조회

### Sparse Index

- GSI를 생성할 때, **모든 항목이 아닌 일부 항목에만 존재하는 Attribute**를 인덱스 키로 지정하면 해당 항목만 인덱스에 포함됨
- 예시: `status=FAILED` 인 항목에만 Attribute를 부여하고 GSI로 구성 → 실패한 작업만 빠르게 조회 가능하고 인덱스 크기도 최소화

### 설계 원칙 요약

- **Access Pattern 먼저**: 조회 패턴을 먼저 나열하고, 그것을 커버하는 키/인덱스를 설계
- **정규화보다 역정규화**: RDBMS처럼 테이블을 쪼개지 않고, 함께 조회되는 데이터는 같은 항목이나 같은 파티션에 모아두는 것이 효율적
- **핫 파티션 주의**: 특정 파티션 키에 트래픽이 몰리면 처리량 제한에 걸림 → 파티션 키를 고르게 분산되도록 설계
