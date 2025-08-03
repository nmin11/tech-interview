## OSI 7 Layer

<img width="600" height="764" alt="osi-model-7-layers-1" src="https://github.com/user-attachments/assets/4a0139cf-b7ff-47c9-ac7c-96154b4b3f2c" />

### 정의

- OSI(Open Systems Interconnection) 모델은 네트워크 시스템의 통신 방식을 설명
  - 직접적으로는 거의 사용되지 않음
  - 다만 네트워크 구조를 이해하는 데에 효과적
- OSI 계층들은 OSI 모델의 기초

### 1. Physical

- 전기, 광신호 등 물리 매체를 통해 실제 비트 전송
- 케이블, 모뎀, 리피터, 허브 등

### 2. Data Link

- 데이터 패킷을 **framing**해서 네트워크 장치 간 전송
- 2개의 하위 계층
  - LLC(Logical Link Control): 통신 흐름 및 오류 관리
  - MAC(Media Access Control): 장치 간 접근 권한 관리
- 브릿지, 스위치 등

### 3. Network

- 가장 효율적인 경로를 찾아서 데이터를 라우팅하는 역할
- 시작점과 목적지를 구분하기 위한 주소(addressing) 로직을 다룸
- 데이터를 패킷 단위로 나눠서 전송하고, 목적지에서 재조립
- 라우터, IP 등

### 4. Transport

- 장치 간 완전하고 신뢰성 있는 데이터 전송
- 핵심 기능들
  - 오류 및 중복 확인 → 오류 발견시 재전송
  - 포트 번호로 식별되는 주소로의 올바른 전송 보장
  - 패킷 분할 및 순차 전송 → 이후 목적지에서 재조립하며 무결성과 정확성 확인
- TCP(Transmission Control Protocol): 연결 기반의 데이터 전송
- UDP(User Datagram Protocol): 비연결형 데이터 전송

### 5. Session

- 네트워크 장치 간 세션 링크 관리
- 주요 기능: 세션 링크 생성, 장치 간 인증 처리, 세션 유지, 연결 종료 등
- 핵심 컨셉: 데이터 손실을 방지하기 위해 체크포인트와의 동기화
- 관련 프로토콜들
  - RPC(Remote Procedure Call)
  - PPTP(Point-to-Point Tunneling Protocol)
  - SCP(Session Control Protocol)
  - SDP(Session Description Protocol)

### 6. Presentation

- 개념: 패킷 기반의 암호화된 코드를 표현할 수 있는 형태로 내보인다
- 기능들
  - 데이터 변환
  - 문자 코드 번역
  - 데이터 압축
  - 암호화 및 복호화
- HTML, JSON, CSV 등

### 7. Application

- 사용자 애플리케이션이 네트워크와 상호작용하는 방식
- 주요 기능: 요청 전송, 통신 동기화, 인증 처리, 개인정보 보호 등
- 관련 프로토콜들
  - FTP(File Transfer Protocol)
  - SMTP(Simple Mail Transfer Protocol)
  - DNS(Domain Name System)

### 논외: TCP/IP

- ISO 표준의 OSI 모델은 다소 복잡했고, 환영받지 못했음
- TCP/IP 기반의 통신 방식이 현재로서 우위를 점하고 있음
- 장점: 단순하고 실용적, 확장하기에 용이, 다양한 기기 및 프로토콜 지원

Reference

- [bmc - OSI Model: The 7 Layers of Network Architecture](https://www.bmc.com/blogs/osi-model-7-layers)

## 주소창에 URL을 입력하면 벌어지는 일들

### Summary

**Redirect → Cache → DNS → IP Routing → TCP → Request → Response → Render**

### 1. Redirect

- 리다이렉트가 존재할 경우 리다이렉트 진행
- 없으면 그대로 해당 요청 진행

### 2. Cache

- 해당 요청이 캐싱 가능한지 파악
- 이미 캐시되어 있는 요청이라면 캐싱된 값 반환

Browser Cache

- 쿠키, 로컬 스토리지 등을 포함한 캐시
- private 캐시라고도 불림
- 브라우저 자체가 문서들을 보유해두는 것

Shared Cache

- 클라이언트와 서버 사이, 대표적으로 프록시 서버가 캐싱을 하는 것
- "리버스 프록시를 둬서 내부 서버로 포워드한다" 라고도 표현함
- AWS CloudFront, Cloudflare 등

### 3. DNS(Domain Name System)

- 계층적 도메인 구조와 분산된 데이터베이스 활용
- FQDN(Fully Qualified Domain Name)을 IP로 바꿔주는 시스템
- 구성 요소
  - Resolver: DNS 요청을 Name 서버에 물어보고, 그 응답값을 클라이언트에 전달
  - Name 서버: 도메인 이름을 IP 주소로 변환하는 서버

DNS Cache

- 요청된 적이 있는 도메인 이름을 로컬 PC에 자동으로 저장해두는 것
- 브라우저 캐싱과 OS 캐싱이 있음

### 4. IP Routing

- IP를 기반으로 실제 서버 주소를 찾아가는 과정

### 5. TCP Handshake

- TCP 3-way 및 TLS handshake 방식으로 연결
- 이후 드디어 요청 및 응답이 이루어짐

### 6. Redner

- 요청한 컨텐츠를 서버로부터 다운로드
- 받은 데이터를 바탕으로 브라우저 엔진이 브라우저 렌더링 과정을 거침

Reference

- [큰돌의터전 - www.naver.com을 주소창에 치면 무슨 일이 일어날까요?](https://youtu.be/YahjHM9UNCA?si=J_yl-Y_-VXkIKeBh)
