## JWT 개념

RFC 7519 표준의 토큰 기반 인증 방식이다.  
토큰 자체에 사용자 권한 정보 등을 담기 때문에 stateless하게 인증 절차를 거칠 수 있다.

토큰 내부 구성요소

- `Header` : 해시 암호화 알고리즘, 토큰 타입으로 구성됨
- `Payload` : 토큰에 담을 claim 정보
- `Signature` : Header, Payload, Secret Key를 합쳐서 암호화한 값

## OAuth 동작 흐름

**용어 정리**

- Resource Owner: 자원에 대한 접근을 허용하는 Entity
  - 사람이 접속한 경우 end-user를 가리킴
- Resource Server: 보호된 자원들을 호스팅하는 서버, access 토큰을 활용한 요청들을 허용
- Client: Resource Owner를 대신해 보호된 자원에 대한 요청을 수행
- Authorization Server: 인증/인가를 마친 Client에게 access 토큰 발급

**동작 흐름**

추상적인 흐름

```
  +--------+                               +---------------+
  |        |--(A)- Authorization Request ->|   Resource    |
  |        |                               |     Owner     |
  |        |<-(B)-- Authorization Grant ---|               |
  |        |                               +---------------+
  |        |
  |        |                               +---------------+
  |        |--(C)-- Authorization Grant -->| Authorization |
  | Client |                               |     Server    |
  |        |<-(D)----- Access Token -------|               |
  |        |                               +---------------+
  |        |
  |        |                               +---------------+
  |        |--(E)----- Access Token ------>|    Resource   |
  |        |                               |     Server    |
  |        |<-(F)--- Protected Resource ---|               |
  +--------+                               +---------------+
```

만료된 토큰 갱신

```
  +--------+                                           +---------------+
  |        |--(A)------- Authorization Grant --------->|               |
  |        |                                           |               |
  |        |<-(B)----------- Access Token -------------|               |
  |        |               & Refresh Token             |               |
  |        |                                           |               |
  |        |                            +----------+   |               |
  |        |--(C)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(D)- Protected Resource --| Resource |   | Authorization |
  | Client |                            |  Server  |   |     Server    |
  |        |--(E)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(F)- Invalid Token Error -|          |   |               |
  |        |                            +----------+   |               |
  |        |                                           |               |
  |        |--(G)----------- Refresh Token ----------->|               |
  |        |                                           |               |
  |        |<-(H)----------- Access Token -------------|               |
  +--------+           & Optional Refresh Token        +---------------+
```

Authorization Code 부여

```
  +----------+
  | Resource |
  |   Owner  |
  |          |
  +----------+
       ^
       |
      (B)
  +----|-----+          Client Identifier      +---------------+
  |         -+----(A)-- & Redirection URI ---->|               |
  |  User-   |                                 | Authorization |
  |  Agent  -+----(B)-- User authenticates --->|     Server    |
  |          |                                 |               |
  |         -+----(C)-- Authorization Code ---<|               |
  +-|----|---+                                 +---------------+
    |    |                                         ^      v
  (A)  (C)                                         |      |
    |    |                                         |      |
    ^    v                                         |      |
  +---------+                                      |      |
  |         |>---(D)-- Authorization Code ---------'      |
  |  Client |          & Redirection URI                  |
  |         |                                             |
  |         |<---(E)----- Access Token -------------------'
  +---------+       (w/ Optional Refresh Token)
```

Reference

- [RFC 6479 - The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749#section-1.2)
