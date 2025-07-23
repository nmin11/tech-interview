## JVM(Java Virtual Machine)은 무엇인가?

JVM의 핵심적인 역할은 가상 운영체제로서 바이트 코드를 해석하고 실행하는 것이다.  
JVM은 Java 프로그램과 OS 사이에서 중개자 역할을 수행하며, Java 프로그램이 OS에 구애받지 않고 코드를 재사용할 수 있게 해준다.  
또한 JVM은 메모리를 효율적으로 관리하기 위해 GC(Garbage Collector)를 사용하며, 이를 통해 사용하지 않는 객체를 자동으로 해제시킨다.

한 번 작성한 코드는 어디에서든 구동이 가능하다는 "Write once, run anywhere"라는 메시지를 특장점으로 꼽는다.  
하지만 한 번의 컴파일로 실행 가능한 기계어가 바로 만들어지는 것이 아니라, 반드시 JVM을 거쳐야만 하는 것이기 때문에 C나 C++에 비해서 다소 느리다.  
그래도 최근에는 JVM 내부의 최적화된 JIT(Just-In-Time) 컴파일러를 통해서 속도의 격차는 현저하게 줄어들고 있다.

Reference

- [자바가상머신, JVM이란 무엇인가?](https://asfirstalways.tistory.com/158)
- ["JVM이란 무엇인가" 자바 가상 머신 이해하기](https://www.itworld.co.kr/news/110837)

## GC(Garbage Collector)의 동작 방식

JVM의 메모리 영역은 다음과 같다.

- `Stack` : 정적으로 할당된 메모리 영역
- `Heap` : 동적으로 할당된 메모리 영역

Heap 영역에는 Object 타입의 데이터를 할당하고, 해당 Object를 가리키는 참조 변수를 Stack 영역에 할당한다.

Garbage Collector는 먼저 Stack 영역의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지를 찾아서 마킹한다.  
그리고 마킹한 변수들이 참조하고 있는 객체들 또한 찾아내서 마킹한다.  
마킹 과정 이후에는 마킹되지 않는 객체들을 Heap에서 제거하는 방식으로 GC가 동작한다.  
이 일련의 과정을 통틀어서 **Mark & Sweep** 과정이라고도 부른다.

Reference

- [👌던의 JVM의 Garbage Collector](https://youtu.be/vZRmCbl871I?si=yN0Q22MZMxLSlaYO)
