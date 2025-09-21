## JVM 정의

*Java Virtual Machine*

- 핵심 역할: 가상 운영체제로서 바이트 코드를 해석하고 실행하는 것
- Java 프로그램과 OS 사이에서 중개자 역할 수행 → Java 프로그램이 OS에 구애받지 않고 코드를 재사용할 수 있게 해줌
- GC(Garbage Collector)를 통해 사용하지 않는 객체를 자동으로 해제
- **"Write once, run anywhere"**
  - 한번 작성한 코드는 어디에서든 작동한다는 모토
  - 다만 컴파일 한번으로 기계어가 바로 만들어지지 않고 반드시 JVM을 거쳐야 하므로 C나 C++에 비해서 다소 느림
  - 그래도 최근 JIT(Just-In-Time) 컴파일러를 통해 속도 격차를 현저히 줄이고 있음

Reference

- [자바가상머신, JVM이란 무엇인가?](https://asfirstalways.tistory.com/158)
- ["JVM이란 무엇인가" 자바 가상 머신 이해하기](https://www.itworld.co.kr/news/110837)

## GC 동작 방식

*Garbage Collector*

JVM의 메모리 영역

- `Stack` : 정적으로 할당된 메모리 영역
  - Object 타입의 데이터 할당
- `Heap` : 동적으로 할당된 메모리 영역
  - Stack에 있는 Object를 가리키는 참조 변수 할당

**Mark & Sweep 동작 과정**

1. Stack 영역의 모든 변수를 스캔 → 각자 어떤 객체를 참조하고 있는지 마킹
2. 마킹한 변수들이 참조하고 있는 객체들 또한 찾아내서 마킹
3. 마킹 과정 이후, 마킹되지 않는 객체들을 Heap에서 제거

Reference

- [10분 테코톡 - 👌던의 JVM의 Garbage Collector](https://youtu.be/vZRmCbl871I?si=yN0Q22MZMxLSlaYO)

## Autoboxing / Unboxing

*Wrapper 클래스가 제공하는 주요 기능*

- Java 5 버전부터 등장하여 원시형 ↔︎ 래퍼 변환을 수동으로 하지 않도록 지원
- Autoboxing: 원시형 타입을 래퍼 클래스 객체로 자동 변환
- Unboxing: 래퍼 클래스 객체를 원시형 타입으로 자동 변환

Autoboxing 예시

```java
int num = 25;
Integer obj = num;
```

→ 컴파일 시점에서의 실제 코드

```java
Integer obj = Integer.valueOf(num);
```

Unboxing 예시

```java
Integer obj = 30;
int num = obj;
```

→ 컴파일 시점에서의 실제 코드

```java
int num = obj.intValue();
```

※ Unboxing은 래퍼 객체와 원시형 값을 섞어서 연산할 때도 발생

```java
Integer obj = 50;
System.out.println(obj + 10); // Unboxing 발생
```

⚠️ 래퍼 객체는 원시형 값에 비해 더 많은 메모리 공간을 차지하므로, 암시적인 Autoboxing 작업의 성능 이슈에 주의해야 한다.

Reference

- [Alexander Obregon - How Java’s Autoboxing and Unboxing Work](https://medium.com/@AlexanderObregon/how-javas-autoboxing-and-unboxing-work-78d9ebcdf4d1)
