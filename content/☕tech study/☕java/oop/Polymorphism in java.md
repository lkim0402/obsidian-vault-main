
> [!polymorphism]
> The ability of a single interface or method to behave differently depending on the object that is calling it

- linked with [[Inheritance in java|inheritance]]
- [[Polymorphism in python]]

| Concept            | Type                      | Timing          | Example                                    |
| ------------------ | ------------------------- | --------------- | ------------------------------------------ |
| Method Overloading | Compile-time Polymorphism | At compile time | Same method name with different parameters |
| Method Overriding  | Runtime Polymorphism      | At runtime      | Subclass redefines a superclass method     |
# Method Overloading 
>You can create different methods with the same name within the same class
>- Can make the same method do different things 
- Compiler knows which method to call based on arguments
- Happens within the same class
- Polymorphism at compile time

```java
public class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        Calculator c = new Calculator();

        System.out.println(c.add(2, 4));        // test 1
        System.out.println(c.add(2, 4, 8));     // test 2
        System.out.println(c.add(3.14, 2.54));  // test 3
    }
}

// println is also an example
System.out.println(3.14);     // 소수 파라미터
System.out.println("hello");  // 문자열 파라미터 
```

- Conditions
	- Have the same name
	- The parameter type/number of parameters should be different
		- The return type DOES NOT MATTER
- Familiar examples
	- `System.out.println`
# Method Overriding (재정의)
> subclass redefines a method from its superclass with the same name, return type, and parameters, but with a different behavior 
- [[JVM, JRE, JDK & the Compilation Process#Java Virtual Machine (JVM)|JVM]] decides at runtime which function to call
- Happens **across a parent-child relationship** - [[Inheritance in java]]
- **Polymorphism at runtime**

```java
// Superclass
public class Animal {
    public void speak() {
        System.out.println("Animal speaks");
    }
}

// Subclass Dog
public class Dog extends Animal {
    @Override
    public void speak() {
        System.out.println("Dog barks");
    }
}

// Subclass Cat
public class Cat extends Animal {
    @Override
    public void speak() {
        System.out.println("Cat meows");
    }
}

```
- add the `@Override` annotation optionally
	- recommended, but not mandatory
	- increased readability for humans lol
	- checks for errors if u used the overriding correctly
- Conditions
	- method name, parameters, return type should ALL be equal
	- [[Access modifiers (접근 제어자)]] should be same or wider

| 조건  | 설명                                                         |
| --- | ---------------------------------------------------------- |
| 1   | **메서드 이름, 매개변수, 반환 타입**이 부모 메서드와 완전히 같아야 한다                |
| 2   | **접근 제어자**는 부모보다 같거나 더 넓은 범위여야 한다 (`private` → ❌ 오버라이딩 불가) |
| 3   | **예외 선언**은 부모보다 같거나 더 구체적인 예외만 가능                          |
| 4   | `final`로 선언된 메서드는 오버라이딩 불가능                                |
| 5   | `static` 메서드는 오버라이딩이 아니라 **숨김(hiding)**                    |

#### Dynamic Binding (동적 바인딩)
- only works for methods that have been overrided
```java
class Animal {
    public void cry() {
        System.out.println("동물이 울음소리를 냅니다.");
    }
}

class Tiger extends Animal {
    @Override
    public void cry() {
        System.out.println("호랑이가 울음소리를 냅니다. 어흥~~~~");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Tiger();
        animal.cry(); // "호랑이가 울음소리를 냅니다. 어흥~~~~"
    }
}
```

# `instanceof`
- You can use this to check if you can typecast
- use this before you upcast/downcast
```java
for (Animal animal : zoo) {
    if (animal instanceof Dog) {
        Dog dog = (Dog) animal;
        dog.fetchStick();
    }
}
```
- upcasting
	- going to the class above, no need to explicitly typecast
	- always possible
- downcasting
	- you need to explicitly cast
	- information loss

| Category            | Up-casting (업캐스팅)                                                        | Down-casting (다운캐스팅)                                                      |
| :------------------ | :----------------------------------------------------------------------- | :------------------------------------------------------------------------ |
| **Direction**       | Subclass → Superclass                                                    | Superclass → Subclass                                                     |
| **Type Conversion** | Implicit (type cast operator can be omitted)                             | Explicit (type cast operator is mandatory)                                |
| **Purpose**         | Utilize polymorphism to treat multiple sub-objects as the same supertype | Access unique subclass functionalities via a supertype reference variable |
| **Safety**          | Safe (no issues at compile-time or runtime)                              | Dangerous (can throw `ClassCastException` on incorrect casting)           |
| **Example**         | `Vehicle vehicle = new Bike();`                                          | `((Bike) vehicle).giveRide();`                                            |
