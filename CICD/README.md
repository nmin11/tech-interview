## Blue-Green 배포 전략

### 개념

- 트래픽을 한번에 구버전에서 신버전으로 옮기는 방식
- 기존 Blue 서버와 함께 Green 서버를 배포하고, Green 서버 배포가 완료되면 Blue 서버는 제거 혹은 유지
- Blue와 Green 환경은 최대한 동일해야 함
- Green 환경 배포 후 Blue 환경은 stage 용도로 활용 가능
  - 다음 배포 때 Blue 환경에 다시 prod를 배포하는 식으로 교체하며 주기적으로 순환하게 할 수도 있음

### 장점

- 다운타임이 거의 없음
- 문제 발생 시 즉시 Blue 환경으로의 롤백이 가능

### 단점

- 서버 리소스를 2배로 가용해야 함

Reference

- [Martin Fowler - Blue Green Deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html)
- [hudi.blog - 무중단 배포 아키텍처와 배포 전략](https://hudi.blog/zero-downtime-deployment)

## Canary 배포 전략

### 개념

- 트래픽을 점진적으로 구버전에서 신버전으로 옮기는 방식
- 유저를 두 그룹으로 나눠서 한 쪽은 구버전, 다른 쪽은 신버전을 사용하게 함
- 롤링 방식: 서버가 여러 대인 경우, 한대씩 카나리 테스트를 진행하며 점진적으로 전환
- 병렬 방식: 두 버전의 리소스를 각각 배포하고 신버전에 대한 트래픽 퍼센티지를 점진적으로 높임

### 장점

- A/B 테스트: 사용자가 두 대안 중 어느 것이 나은지 판별하게 할 수 있음
- 점진적인 트래픽 과부하를 활용한 성능 테스트 가능
- 문제 발생 시에도 기존 서버가 유지되고 있으므로 언제든지 롤백 가능

### 단점

- 카나리 그룹의 사용자가 예상치 못한 버그를 먼저 경험하게 될 수 있음
- 트래픽 분할 로직 및 모니터링 시스템 필요

Reference

- [Tomas Fernandez - What Is Canary Deployment?](https://semaphore.io/blog/what-is-canary-deployment)
- [hudi.blog - 무중단 배포 아키텍처와 배포 전략](https://hudi.blog/zero-downtime-deployment)
