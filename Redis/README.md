## 싱글 스레드로 만들어진 이유

**성능 최적화, 복잡성 감소, 데이터 일관성 유지**

- 멀티 스레드 환경의 동시성 문제(Race Condition, Deadlock)를 피할 수 있음
- 동시성 제어 없이도 데이터의 일관성을 유지할 수 있음
- 인메모리 기반으로 대부분의 요청을 나노초 단위로 수행할 수 있기 때문
- 단일 스레드의 이벤트 루프를 사용하므로 컨텍스트 스위칭 비용도 최소화
  - 이벤트 루프는 비동기적으로 여러 클라이언트의 요청 처리

### Redis 6.0의 I/O 멀티스레딩

<img width="801" height="318" alt="redis-6 0" src="https://github.com/user-attachments/assets/06300622-ccbd-410e-9768-8d885702a6be" />

**I/O 멀티플렉싱 = 네트워크 I/O 부분에 대한 멀티 스레드 지원**

- 단일 스레드를 사용하지만 여러 클라이언트의 요청을 차단하지 않도록 할 수 있게 해줌
- 단일 스레드가 여러 I/O 스트림들을 모니터링할 수 있게 해주는 방식
- `select` `poll` `epoll` 등의 시스템 호출을 활용해서 애플리케이션이 이벤트를 처리하도록 하는 구조
- 이를 통해 메모리 뿐만 아니라 CPU, 네트워크 등 하드웨어 리소스도 활용할 수 있게 되었음

Reference

- [LinkedIn - Why the heck Single-Threaded Redis is Lightning fast? Beyond In-Memory Database Label](https://www.linkedin.com/pulse/why-heck-single-threaded-redis-lightning-fast-beyond-in-memory-kapur)
