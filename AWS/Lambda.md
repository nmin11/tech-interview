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
