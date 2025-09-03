## RESTful API

**정의**

- REST 아키텍처 스타일에 따라 설계된 API
- REST = Representational State Transfer(표현적 상태 전송)
- Roy Fielding이 2000년 제안, 웹의 구조적 원칙을 API 설계에 적용한 것
- RESTful API = HTTP 기반에서 리소스 중심으로 통신하며, **표현(Representation)** 과 함께 **상태(State)** 를 전송하는 방식

**구현 방식**

- 자원 = URI
- 상태 = HTTP Method
- 표현 = Header

**REST API 설계 원칙**

- Client/Server : 클라이언트와 서버의 책임 분리
- Stateless : 클라이언트는 상태를 저장하지 않음, 요청에 필요한 모든 정보가 있다
- Cache : 응답은 캐싱 가능
- Uniform interface
  - 요청 내 **자원(Resource)** 식별 : 개별 자원들은 URI를 통해 식별되어야 함, 자원은 **표현(Representation)** 개념과는 별개
  - 자원은 표현에 의해 조작됨 : 자원은 표현을 통해 수정 및 삭제될 수 있도록 메타데이터 등 충분한 정보를 가진다
  - 자체로 설명적인 메시지 : 모든 메시지는 메시지를 처리하기 위해 충분한 정보를 가진다
  - HATEOAS(Hypermedia As The Engine Of Application State)
    - REST 애플리케이션에 접속하면 서버에서 제공하는 동적 링크를 사용해 모든 접속 가능한 자원을 검색할 수 있어야 한다
    - 자원들에 대한 액세스가 진행됨에 따라, 서버는 다른 자원에 접근할 수 있는 하이퍼링크가 포함된 텍스트를 응답해야 한다
    - 클라이언트는 서버 구조 관련 정보를 하드 코딩할 필요는 없다
- Layered system : 클라이언트는 end 서버나 중개점을 일반적으로 알 수 없다
- Code on demand (optional) : 서버는 클라이언트에게 표준 가상 머신에서 실행될 수 있는 임시 로직 전송 가능

Reference

- [Wikipedia - REST](https://en.wikipedia.org/wiki/REST)
