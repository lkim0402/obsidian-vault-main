```java
@FunctionalInterface
interface Calculator {
    int sumTwoNumber(int x, int y);
}

public class Application {
    public static void main(String[] args) {
        Calculator c3 = (x, y) -> x + y;
        System.out.println("40과 50의 합은 : " + c3.sumTwoNumber(40, 50));
    }
}

```
- put a functional interface (함수형 인터페이스)

```java
@FunctionalInterface
interface ExampleFunction {
    int sum(int num1, int num2);
}

public class LamdaExample1 {
    public static void main(String[] args) {
        ExampleFunction exampleFunction = (num1, num2) -> num1 + num2;
        System.out.println(exampleFunction.sum(10, 15));
    }
}

// 출력값: 25
```

- 함수형 인터페이스는 하나의 기능만을 명확하게 정의하기 위한 용도이기 때문에, 두 개 이상의 추상 메서드가 존재하면 컴파일 에러가 발생하거나 람다식으로 사용할 수 없다.
- **`@FunctionalInterface` 어노테이션을 붙이면 컴파일러가 규칙을 강제한다.**
    - 이 어노테이션은 선택 사항이지만, 붙여두면 해당 인터페이스가 실제로 함수형 인터페이스 규칙을 따르고 있는지 컴파일 시점에 검사하게 된다.
- **람다식은 인터페이스의 추상 메서드와 정확히 1:1로 매칭되어야 한다.**
    - 함수형 인터페이스는 하나의 추상 메서드만 가지므로, 람다식은 해당 추상 메서드의 시그니처(매개변수 타입, 개수, 리턴 타입 등)와 정확히 일치해야 한다.

![[Pasted image 20250528100643.png]]

```java
Calculator cal3 = (a,b) -> a + b;
System.out.println(cal3.sumTwoNumber(10,10));
```
- 알아서 매핑이 됨

- lambda에서 외부 변수는 final, effective final만 사용가능


- 왜 람다는 functional interface를 써야만 하느냐
	- 람다는 인스턴스를 만드는거임
	- 근데 람다는 abstract function을 1개 밖에 override 못함
	- 그래서 functional interface (1 abstract method) is a must use for lambda 
- 이름이 없는 익명 객체가 variable에 assign 되는거임

![[{474999BE-CCB6-45E5-91C8-787D4CEFA617}.png]]

