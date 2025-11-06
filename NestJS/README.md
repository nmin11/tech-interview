## `@Injectable` 작동 원리

### 동작 원리

- NestJS는 IoC 컨테이너를 통해 의존성 주입(DI) 시스템 제공
- `@Injectable()` 데코레이터가 적용된 클래스는 Provider로 등록됨
- Module의 `providers` 배열에 등록된 Provider는 IoC 컨테이너에 의해 관리
- Reflect Metadata API를 활용하여 생성자 매개변수의 타입 정보 추출
- NestJS는 런타임에 메타데이터를 읽어 의존성 그래프를 구성하고 자동으로 인스턴스 생성 및 주입

### Scope 설정

- `DEFAULT`: 싱글톤으로 동작하며, 애플리케이션 전체에서 하나의 인스턴스만 생성되어 공유됨
- `REQUEST`: HTTP 요청마다 새로운 인스턴스가 생성되며, 요청이 끝나면 소멸됨
- `TRANSIENT`: 주입될 때마다 새로운 인스턴스가 생성됨
