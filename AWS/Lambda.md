## Lambda의 장단점

### 장점

- 비용 절감
  - 밀리초 단위 CPU 시간으로 비용이 청구되므로 비용을 크게 절감할 수 있음
  - 매월 첫 100만 건 요청 무료
- 인프라 관리의 용이성
  - 서버 스케일링 및 비용 최적화 작업이 필요 없음
  - Lambda 함수를 빌드 및 배포하는 과정도 비교적 단순함
- 대규모 애플리케이션에도 적합
  - 부하가 큰 애플리케이션의 경우 Lambda는 여러 인스턴스를 동시에 프로비저닝하고 실행

### 단점

- Cold Start
  - 임시 컨테이너를 생성하고 종속성 배포 및 코드 실행하기까지 시간이 소요됨
- 실행 시간 제한
  - 최대 실행 시간이 15분이기 때문에 장기 실행 프로세스를 처리할 수 없음
- 리소스 제약
  - 10GB 메모리, 6개 vCPU, 10GB 임시 스토리지로 구성되어 있음
  - 높은 컴퓨팅 성능이나 대용량 데이터셋이 필요한 경우에 부적합

Reference

- [Ashish Saxena - Pros and Cons of AWS Lambda](https://www.linkedin.com/pulse/pros-cons-aws-lambda-ashish-saxena)

## Lambda에서 DB 커넥션 풀 관리 방법

### 문제점

- 사실 서버리스 함수와 관계형 데이터베이스는 잘 맞지 않음
- 서버리스는 매우 짧은 수명을 가지는 반면, 전통적인 관계형 데이터베이스는 지속되는 커넥션을 사용하도록 설계되었기 때문

### Amazon RDS Proxy

- 애플리케이션과 RDS 사이에서 중개자 역할 수행
- 커넥션 풀을 제한하고, 재사용 가능하게 해줌
- IAM, Secrets Manager와 함께 자격 증명을 안전하게 제공
- Lambda 함수는 RDS에 직접 연결하지 않고, RDS Proxy에 연결하도록 하면 됨

Reference

- [Amazon RDS Proxy](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/rds-proxy.html)
- [QloudX - Pool & Share Database Connections in Lambda Functions: Use RDS Proxies!](https://www.qloudx.com/pool-share-database-connections-in-lambda-functions-use-rds-proxies)
