## `==` 연산과 `equals()` 차이

**`==` 연산자**

- 원시 타입인 경우 : 실제 값 비교
- 참조 타입인 경우 : 참조값(메모리 주소) 비교
  - 동일한 객체인지 여부를 확인할 수 없음

**`equals()` 메서드**

- 기본 `Object.equals()` : 참조값 비교
- String, Integer 등 많은 클래스에서는 content equality(논리적 동등성)를 비교하도록 구현됨

Reference

- [GeekssforGeeks - Difference Between == Operator and equals() Method in Java](https://www.geeksforgeeks.org/java/difference-between-and-equals-method-in-java)

## `static` 키워드의 동작 방식

**정의**

- 클래스 단위의 멤버를 정의
- 인스턴스가 아닌 클래스에 속함을 의미

**적용 대상 및 작동 방식**

class 내에서 다음 4가지 대상에 적용 가능

- 변수
  - 클래스가 로드될 때 단 한 번 생성됨, 클래스의 모든 인스턴스가 공유
- 메서드
  - 인스턴스 없이 클래스 이름으로 호출 가능 `ClassName.method()`
  - `this`, `super`를 참조할 수 없음
  - static 데이터에 접근하거나, 다른 static 메소드를 호출하는 것만 가능
- 초기화 블록
  - 클래스가 로드될 때 한 번만 실행되는 블록
  - 주로 복잡한 초기화 로직 처리 시 사용
- 중첩 클래스
  - 외부 클래스의 인스턴스 없이 독립적으로 생성 및 사용 가능
  - 외부 클래스의 static 멤버만 접근 가능

**장점**

- 메모리를 한번만 사용하므로 메모리 효율적
- 일반 멤버보다 빠른 엑세스 가능
- 인스턴스 생성 여부와 관계 없이 어디서든 접근 가능
- `static final` 변수는 프로그램 전체에서 사용할 수 있는 상수로 활용 가능

**단점**

- 재정의 및 동적 바인딩 불가
- static 변수와 static 메소드는 강결합되어 단위 테스트가 어려움
- static 변수는 프로그램이 실행되는 동안 계속 유지되므로 불필요한 메모리 차지가 될 수 있음
- 은닉화, 상속 등 OOP의 이점을 살리기 어려워짐

Reference

- [GeeksforGeeks - static Keyword in Java](https://www.geeksforgeeks.org/java/static-keyword-java)
