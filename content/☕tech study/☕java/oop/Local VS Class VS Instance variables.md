|                                 | **Declaration Location (선언 위치)** | **Static? (static 여부)** | **Memory Area (메모리 영역)** | **Creation Time (생성 시점)** | **Destruction Time (소멸 시점)** |
| ------------------------------- | -------------------------------- | ----------------------- | ------------------------ | ------------------------- | ---------------------------- |
| **Class Variable (클래스 변수)**     | Class body (클래스 영역)              | Yes (`static`) 있음       | Method Area (클래스 영역)     | Class loading (클래스 로딩 시)  | Program ends (프로그램 종료 시)     |
| **Instance Variable (인스턴스 변수)** | Class body (클래스 영역)              | No 없음                   | Heap (힙 영역)              | Object creation (객체 생성 시) | GC (객체가 GC될 때)               |
| **Local Variable (지역 변수)**      | Inside method (메서드 내)            | Irrelevant 상관없음         | Stack (스택 영역)            | Method call (메서드 호출 시)    | Method end (메서드 종료 시)        |
- [[Class vs Instance variables]]