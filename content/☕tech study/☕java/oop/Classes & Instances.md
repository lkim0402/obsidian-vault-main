# Class
>[!class]
>A unit of code that is the basic building block of Java programs. 
>- In Java, nothing can exist outside of a class. A Java program is stored in a class.
>- Convention: starts with capital letter, and class & file name must match
>- `public class <name> { ... }`

- Class header
	- The first line in a class, like `public class <name> {`
- Two types of members in a class:
	- Fields
	- Methods
		- A program unit that represents a particular action or computation.
		- The next-smallest unit of code in Java after classes 
		- The first line of a method = *method header*
- For getter and setter, you can use the `Generate` function in Intellij

```java
class Person 
{
    String name;     // field
    void greet() {   // method
        System.out.println("Hello, World!");
    }
}
```
## `Main` method
- At a minimum, a complete program requires a special method named `main`.
- `<statement>` = An executable snippet of code that represents a complete command.
	- `System.out.println("Hello, world!")` is a `println` statement
	- They are executed *in order*
```java
public static void main(String[] args) {
	<statement>; // series of actions
	<statement>;
	...
	<statement>;
}
```

## `Object` class
- By default, every class extends an `Object` class
	- Every time you try to print an object, it will call the `toString()` method of the object
```java
Person p = new Person(); // creating a new instance
```

| **Component**   | **구성 요소** | **Description** / **설명**                                                                       |
| --------------- | --------- | ---------------------------------------------------------------------------------------------- |
| **Field**       | 필드        | - Attributes or properties an object has (e.g., name, age, etc.)<br>- 객체가 가지는 속성 (예: 이름, 나이 등) |
| **Method**      | 메서드       | - Actions or behaviors the object can perform<br>- 객체가 수행할 수 있는 동작                             |
| **Constructor** | 생성자       | - A special method called when the object is created<br>- 체 생성 시 호출되는 특수한 메서드                  |
| **Inner Class** | 이너 클래스    | - A class defined inside another class, used when needed<br>- 클래스 내부에 정의된 또 다른 클래스 (필요 시 사용)   |
> 🧠 **Fields** and **methods** represent the **state** and **behavior** of an object.  
> - state -> represented by fields
> 	- the data an object holds
> - behavior -> represented by methods
> 	- the actions an objects can perform

## Fields
- Variables that exist to hold the data the instance will have, and they represent attributes
	- Class and instance variables are both fields
	- [[Class vs Instance variables]]
- related: [[Local VS Class VS Instance variables]]
## Methods
- What's in a method
	- Method signature: method name + parameters
		- `addInt(int num1, num2)`
		- A good method signature lets you know what the function does & returns at first glance
	- Method body
- [[Class vs Instance methods|class methods]]
- [[Polymorphism in java#Method Overloading|method overloading]]
#### Variadic Arguments; varargs
- Allows a method to accept **zero or more arguments** of a specified type — like passing an array without needing to create one manually
- without this, we'll have to overloading `n` times
```java
public void printNames(String... names) {
    for (String name : names) {
        System.out.println(name);
    }
}

printNames(); // zero arguments
printNames("Alice");
printNames("Bob", "Charlie", "Dave");
```
- Conditions
	- **Only one varargs parameter** is allowed per method
	- It **must be the last parameter** in the parameter list
- Internally
	- Java converts to array

# Instance
>[!Instance]
>An instance is a real instance of a class.
>- It has its own data stored in the attributes.
>- You can call methods on the object to perform actions or change its state

- All instances are objects
```java
Person p1 = new Person();
```
- p1 is a reference variable, which stores the address of the heap that has the data
- You "instantiate"  with the `new` keyword
	- `new` 쓰는 건 생성자를 호출하는 것
	- `new` => you're putting the object in the heap memory and putting its address in the reference variable
- Image overview
	![[JVM_class.png]]
	- 즉 같은 클래스로 만든 모든 객체는 동일한 메서드 값을 공유하기 때문에 여러 번 같은 메서드를 선언해 주는 것이 아니라 한 번만 저장해 두고 필요한 경우에만 클래스 영역에 정의된 메서드를 찾아 사용할 수 있는 것입니다.
	- All instances of the same class share the same methods
		- they all reuse the same method stored in the class area
