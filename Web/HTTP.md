## HTTP와 HTTPS의 차이

**HTTP**

- TCP/IP 위에서 작동하는 stateless 프로토콜
- 암호화되지 않은 평문 데이터를 전송하는 프로토콜

**HTTPS**

- HTTP에 암호화를 추가한 프로토콜
- HTTPS 적용을 위해서는 CA(Certificate Authority) 기업으로부터 인증을 받아야 함
  - 대부분의 브라우저에는 CA 기업들의 공개 키들이 내장되어 있음

## HTTPS 작동 방식

![HTTPS 통신 과정](https://user-images.githubusercontent.com/75058239/166633303-b891862f-4246-4f7c-aa8a-628f850faac5.png)

- 1단계: 클라이언트에서 사용 가능한 암호화 알고리즘 및 TLS 리스트 전송
- 2단계: 서버에서 사용할 암호화 알고리즘 및 TLS를 정하고 난수값 전송
- 3단계: 서버에서 CA로부터 발급 받은 인증서 전송
- 4단계: 서버에서 키 교환에 필요한 정보 제공 (암호화 알고리즘에 따라 생략 가능)
- 5단계: 클라이언트의 인증서 요청
- 6단계: 클라이언트에서 대칭키(`pre-master-key`) 생성 후 공개키 방식으로 전송
- 7단계: 서버에서 대칭키 복호화 후 `master-key` 생성

## Cookie & Session

### 개요

- HTTP는 stateless 하므로 유저의 행동을 추적하고 정보를 유지할 수 있는 수단 필요
- 쿠키와 세션을 활용하면 HTTP 프로토콜에서도 상태를 저장할 수 있음

### Key Difference

|      구분      |        Cookie        |        Session        |
| :------------: | :------------------: | :-------------------: |
|   저장 위치    |     client-side      |      server-side      |
|     의존성     |        독립적        |     쿠키에 의존적     |
|    생명주기    | 라이프타임 설정 가능 | 브라우저 종료 시 끝남 |
|   저장 공간    |       4KB 제한       |        넉넉함         |
|  해제 명령어   |         없음         |         있음          |
|      보안      |         낮음         |         높음          |
|      성능      |         빠름         |         느림          |
| 인증 작업 방식 |       JWT 활용       |    세션 기반 인증     |

### Cookie

- 브라우저에 key-value 형태로 저장
- `HttpOnly` `Secure` `SameSite` 옵션과 함께 보안을 강화할 수 있음
- 사용자별 설정 및 선호도 저장 용도
- stateless 인증을 위한 JWT 관리 용도
- 일반적으로 세션 추적, 로그인 상태, 사용자 경험 개인화 등을 위해 사용됨

### Session

- 고유 세션 ID를 활용해 user-specific 정보들을 다룸
- 사용자가 웹 애플리케이션에서 지속적으로 상호작용하는 상태를 관리
- 쿠키에 세션 ID를 저장해서 서버의 세션을 식별하는 데에 활용
- 유저 데이터가 여러 페이지에 걸쳐 유지되는 상태 저장 용도
- 사용자별 임시 데이터 저장소가 필요할 때 사용
- 사용자 정보에 대한 은닉이 필요한 경우 사용

Reference

- [GeeksforGeeks - Difference Between Session and Cookies](https://www.geeksforgeeks.org/javascript/difference-between-session-and-cookies)
- [GURU99 - Difference between Session and Cookies](https://www.guru99.com/difference-between-cookie-session.html)
