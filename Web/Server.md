## Web Server 와 WAS

### Web Server

- HTTP(S) 요청을 받아 **정적 컨텐츠(HTML, CSS, 이미지 등)** 를 반환하는 서버
- 동적 컨텐츠 요청 시 WAS로 요청을 전달할 수 있음
- CGI(Common Gateway Interface)를 활용해서 동적 컨텐츠 제공 기능을 넣을 순 있음
- Apache, Nginx, IIS 등

### WAS(Web Application Server)

- 웹 서버와 동일하게 HTTP(S) 기반으로 동작
  - 웹 서버가 할 수 있는 대부분의 기능도 처리 가능
  - 그밖에 RPC, SOAP 등 다양한 프로토콜도 지원
- 주요 임무: **서블릿 컨테이너 기능 제공**, **동적 컨텐츠 생성**, **애플리케이션 로직 실행**
- Tomcat, JBoss, Jeus, WebSphere 등

### 그러면 WAS만 써도 되지 않나?

- WAS의 과부하를 줄이고, 중요한 비즈니스 로직에 집중할 수 있게 해야 한다
- 따라서 웹 서버를 분리해서 정적 콘텐츠를 제공하도록 하는 방식이 유용

Reference

- [요즘IT - 웹 서버와 WAS](https://yozm.wishket.com/magazine/detail/1780/)
