## `==` 연산과 `equals()` 차이

**`==` 연산자**

- 원시 타입인 경우 : 실제 값 비교
- 참조 타입인 경우 : 참조값(메모리 주소) 비교 → 동일한 객체인지 여부 확인ㄴ

**`equals()` 메서드**

- 기본 `Object.equals()` : 참조값 비교
- String, Integer 등 많은 클래스에서는 content equality(논리적 동등성)를 비교하도록 구현됨

Reference

- [GeekssforGeeks - Difference Between == Operator and equals() Method in Java](https://www.geeksforgeeks.org/java/difference-between-and-equals-method-in-java)
