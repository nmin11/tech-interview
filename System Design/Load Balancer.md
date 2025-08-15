## 로드밸런싱

### 개념

- 네트워크 트래픽을 여러 서버로 분산해서 애플리케이션 가용성을 최적화하는 방식
- 다중 서버 중 하나가 오프라인 상태가 되어도 서비스는 지속된다

### 구성 방식별 구분

**소프트웨어 방식**

- 애플리케이션 형태 혹은 서버의 구성요소로 구현되어 유연하고 적용하기에 용이

**하드웨어 방식**

- 4계층/7계층 라우터라고도 알려져 있음
- HTTP, HTTPS, TCP 및 UDP까지 모든 트래픽 유형 처리 가능
- 구성 및 적용 비용이 높기 때문에 첫번째 진입점에 요청이 많이 몰리는 경우에만 사용됨

**가상 방식**

- VM 혹은 가상 환경의 소프트웨어를 활용하는 방식

### 기능별 구분

**Layer 4 (L4) Load Balancer**

- OSI 모델의 전송(transport) 계층에서 작동
  - TCP/UDP 기반 라우팅
- 네트워크 계층의 정보(IP 주소, 포트 번호 등)를 토대로 포워딩 대상을 결정함

**Layer 7 (L7) Load Balancer**

- OSI 모델의 애플리케이션 계층에서 작동
- URL, 헤더, 쿠키 등을 활용해서 로드밸런싱을 할 수 있음

**Global Server Load Balancing (GSLB)**

- 다중 데이터 센터 혹은 지리적으로 분산된 서버 환경에서의 로드밸런싱 수행 가능
- 접근성, 지리적 위치, 서버 상태 등을 활용해 지능적으로 트래픽 분산

### 주요 로드밸런싱 알고리즘들

![loadbalance-algorithms](https://github.com/user-attachments/assets/5bfa0e4c-5e77-46d7-be75-5a05d89a8d75)

**Round Robin**

- 요청을 순차적, 순환적으로 분산하는 간단한 방식
- 구현이 쉽지만, 서버의 현 부하 상태를 고려하지 않는다는 단점이 있음

**Weighted Round Robin**

- 라운드 로빈과 유사하지만, 리소스마다 가중치 점수를 부여해서 이에 따라 트래픽을 차등 분산

**Source IP Hash**

- 요청한 IP 주소의 해시값을 기반으로 분산
- 동일한 IP 주소로 요청하면 계속 같은 서버를 활용할 수 있음을 보장해줌

**Least Connection Method**

- 커넥션 수가 가장 적은 서버를 동적으로 계산해서 로드밸런싱

**Least Response Time Method**

- 응답 시간이 가장 빠른 서버를 동적으로 계산해서 로드밸런싱

Reference

- [GeeksforGeeks - Load Balancer - System Design Interview Question](https://www.geeksforgeeks.org/system-design/load-balancer-system-design-interview-question)
