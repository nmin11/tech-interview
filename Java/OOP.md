## `abstract class` & `interface`

### 공통점

- 상속 받는 자식 클래스는 부모의 메서드를 구현하는 것이 강제됨

### 차이점

- `abstract class` : 상속 받은 기존 로직을 이용하면서 확장하는 방식
  - 단일 상속만 가능
- `interface` : 껍데기 뿐인 함수에 대한 구현 강제
  - 다중 상속도 가능

### Interface default method

```java
interface Drawable {
    void draw();
    
    default void print() {
        System.out.println("Printing...");
    }
}
```

- Java 8 부터 도입된 인터페이스 메소드의 기본 구현체
- 인터페이스 구현 클래스가 추가될 때, 리팩토링을 하지 않고도 구현체가 유지될 수 있도록 도와주는 용도
- `abstract class`
  - 클래스 내 인스턴스 변수, 생성자 정의 가능
  - 메서드 내에서 인스턴스 변수 활용 가능
- `interface default method`
  - 생성자 정의 불가능
  - 메서드 내에서 public static final 변수만 활용 가능

Reference

- [강관우 - 자바의 추상 클래스와 인터페이스](https://brunch.co.kr/@kd4/6)
- [Baeldung - Static and Default Methods in Interfaces in Java](https://www.baeldung.com/java-static-default-methods)
