## JWT 개념

RFC 7519 표준의 토큰 기반 인증 방식이다.  
토큰 자체에 사용자 권한 정보 등을 담기 때문에 stateless하게 인증 절차를 거칠 수 있다.

토큰 내부 구성요소

- `Header` : 해시 암호화 알고리즘, 토큰 타입으로 구성됨
- `Payload` : 토큰에 담을 claim 정보
- `Signature` : Header, Payload, Secret Key를 합쳐서 암호화한 값
