## SSO

_Single Sign-On_

**개념**

- 하나의 인증으로 여러 애플리케이션 및 서비스에 접근할 수 있게 해주는 인증 메커니즘
- 사용자는 한 번만 로그인하면 별도의 추가 인증 없이 연결된 모든 시스템 사용 가능

**용어 정리**

- **IdP (Identity Provider)**: 사용자의 신원을 인증하고 인증 토큰을 발급하는 주체
  - 예: Google, Okta, Azure AD, Keycloak
- **SP (Service Provider)**: 사용자가 실제로 사용하려는 서비스 또는 애플리케이션
  - IdP로부터 받은 인증 정보를 신뢰하여 사용자에게 접근 권한 부여
  - 예: Gmail, Slack, Salesforce

**동작 흐름**

1. 사용자가 SP(서비스)에 접근 시도
2. SP가 사용자를 IdP로 리다이렉트
3. 사용자가 IdP에서 인증 수행 (이미 로그인된 경우 생략)
4. IdP가 인증 토큰(SAML Assertion, JWT 등)을 생성하여 SP에 전달
5. SP가 토큰을 검증하고 사용자에게 접근 권한 부여

**장점**

- 사용자 경험 개선: 여러 서비스 사용 시 반복적인 로그인 불필요
- 비밀번호 관리 부담 감소: 하나의 인증 정보만 관리
- 보안 강화: 중앙 집중식 인증 정책 적용 가능
- 관리 효율성: 사용자 계정 생성/삭제를 한 곳에서 관리

**단점**

- 단일 장애점(Single Point of Failure): IdP 장애 시 모든 서비스 접근 불가
- 보안 위험 집중: 한 번의 인증 돌파로 모든 연결된 서비스 접근 가능
- 초기 구축 복잡도: IdP와 각 SP 간의 신뢰 관계 설정 필요
- 의존성: 외부 IdP 사용 시 서비스 제공자에 대한 의존도 증가

**주요 프로토콜**

- **SAML 2.0**: XML 기반, 엔터프라이즈 환경에서 널리 사용
- **OAuth 2.0**: 주로 인가(Authorization)에 사용되지만 SSO 구현에도 가능
- **OpenID Connect (OIDC)**: OAuth 2.0 위에 구축된 인증 레이어, 최신 표준

## JWT

**개념**

RFC 7519 표준의 토큰 기반 인증 방식이다.  
토큰 자체에 사용자 권한 정보 등을 담기 때문에 stateless하게 인증 절차를 거칠 수 있다.

토큰 내부 구성요소

- `Header` : 해시 암호화 알고리즘, 토큰 타입으로 구성됨
- `Payload` : 토큰에 담을 claim 정보
- `Signature` : Header, Payload, Secret Key를 합쳐서 암호화한 값

**장점**

- stateless 하기 때문에 세션에 저장해둘 필요가 없음
- 토큰의 claim에 자체적으로 사용자 정보를 담고 있으므로, 별도의 추가 데이터 조회 불필요
- base64 인코딩 방식을 활용하므로 Header에 담기 적합한 작은 크기
- 분산 시스템에서 중앙 서버 호출 없이 활용하기에 용이

**단점**

- claim 및 서명이 포함되므로, 일반적인 세션 쿠키에 비해 용량이 큼
- 만료 시간 이전에는 토큰을 취소시키는 메커니즘이 없음
- base64 디코딩을 통해 손쉽게 정보를 탈취당할 수 있음
- 서명 키 노출 시 공격자가 JWT 생성 가능

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
