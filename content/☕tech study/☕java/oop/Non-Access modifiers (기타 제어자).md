>Provide additional meaning, such as static behavior, finality, synchronization, etc.
- they are loaded into memory when the **class is first loaded** (usually when it’s first accessed or used), and **they stay loaded until the class is unloaded** — typically at program termination
- can use multiple of them at the same time

| Modifier   | Applies To                                                   | Meaning (설명)                                                                                    |
| ---------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| `static`   | Fields, methods, blocks, inner classes                       | Belongs to the **class** instead of instances                                                   |
| `final`    | Variables, methods, classes                                  | Cannot be changed/overridden/inherited                                                          |
| `abstract` | Classes, methods<br>- [[Abstraction - Classes & Interfaces]] | Class cannot be instantiated; method has no implementation and must be overridden by subclasses |
(there are more)

# Static Members
- [[Class vs Instance variables#Class variables|Class/static variables]]
- [[Class vs Instance methods|Class/static methods]]
- [[Static blocks]]
- [[Static classes (only inner)]]
- [[Static import]]
- [[Singleton Patterns]]

## Static vs instance members

| 구분     | static 멤버       | 인스턴스 멤버      |
| ------ | --------------- | ------------ |
| 메모리 영역 | 클래스 영역          | 힙 영역         |
| 접근 방식  | 클래스명.멤버         | 객체.멤버        |
| 공유 여부  | 모든 인스턴스 간 공유    | 객체마다 별도 보유   |
| 생성 시점  | 클래스 로드 시        | new 생성 시     |
| 사용 예시  | 공통 속성, 유틸리티 메서드 | 객체의 개별 상태 관리 |

### Static Members Initialization Order
![[member_initialization.png]]
- Instance Initialization Block
	- a block of code inside a class but outside any method or constructor, written simply as `{ ... }`
	- runs **every time an object (instance) of the class is created**, **before the constructor** is executed


#### Static member initialization order:

| 순서  | 단계            | 설명 (Korean)                 | Description (English)                                                 |
| --- | ------------- | --------------------------- | --------------------------------------------------------------------- |
| 1   | JVM 기본값       | static 변수에 자료형에 맞는 기본값이 할당됨 | JVM gives default values to static fields (e.g. `0`, `null`, `false`) |
| 2   | 명시적 초기화       | static 변수에 대입한 초기값 실행       | Static fields with assigned values get initialized                    |
| 3   | static 초기화 블록 | static {} 안의 코드 실행          | `static {}` block runs for more complex logic                         |
```java
class Example {
    static int a;              // 기본값 0
    static int b = 10;         // 명시적 초기화
    static {
        a = 100;               // static 초기화 블록
    }
}
```

