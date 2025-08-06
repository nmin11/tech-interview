## Spring Security 작동 방식

### 기본 개념

- Spring 기반 애플리케이션에 인증/인가, CSRF, CORS 등 보안 기능을 통합 제공
- **FilterChain** 기반으로 작동
- Spring Security를 사용하지 않을 때는 기본적으로 DispatcherServlet이 모든 HTTP 요청을 가로채는 방식으로 작동했음
- Spring Security를 사용하면 요청들이 DispatcherServlet 이전에 FilterChain을 거치게 됨

### username, password 인증 방식의 작동 흐름

1. **FilterChain**이 가장 먼저 요청을 가로챔
2. Authentication Filter를 거침 (ex. UsernamePasswordAuthenticationFilter)
3. 필터는 요청으로부터 username, password를 추출 (HttpServletRequest로부터)
4. 자격 증명과 함께 UsernamePasswordAuthenticationToken 생성
5. `AuthenticationManager.authenticate()` 호출
6. authenticate() 메서드는 **AuthenticationProvider** 중 하나를 활용해 인증 시도
7. 성공적으로 인증에 성공하면, 인증/인가를 포함하는 완전한 UsernamePasswordAuthenticationToken 반환
8. 인증 토큰을 **Spring Security Context**에 설정
9. 인증 절차 완료 후 **DispatcherServlet**으로 요청 전달

### 용어 정리

**FilterChain**

- 모든 요청을 가로채서 인증 작업을 수행할 수 있음
- 각 필터는 다음 필터로 요청을 전달하거나, 요청을 중단하고 응답을 보낼 수 있음

**AuthenticationManager**

- 인증 요청을 처리하는 인터페이스

**ProviderManager**

- AuthenticationManager의 구현체
- 전달된 Authentication 객체  인증이 가능한 AuthenticationProvider를 골라 인증 작업 수행
  - 만약 인증 가능한 AuthenticationProvider가 없는 경우 AuthenticationException 발생

**AuthenticationProvider**

- 사용자 인증 계약을 정의하는 인터페이스
- Authentication 객체를 가져오는 책임을 가짐
- `authenticate()` : Authentication 객체를 받아서 인증 작업
- `supports()` : Authentication 객체를 인증할 수 있는지 여부를 판별
- **DaoAuthenticationProvider**
  - Spring Security가 제공하는 기본 provider
  - UserDetailsService를 사용해 DB에서 사용자 정보를 가져옴

Reference

- [Ayush Singh - Understanding Spring Security Authentication Flow](https://medium.com/%40aprayush20/understanding-spring-security-authentication-flow-f9bb545bd77)
