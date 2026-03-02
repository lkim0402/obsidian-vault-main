# Abstraction
>[!key]
>Hiding the complex implementation and showing only the **essential features**
>- Usually done through abstract classes/interfaces

- useful in database modeling
- interface used a lot more than abstract class
# Abstract Classes
- `abstract`
	- a [[Non-Access modifiers (기타 제어자)|non access modifier]]
	- indicates "incomplete (미완성)"
- Abstract classes
	- u can have normal methods
	- use `extends`
	- It's an abstract class if u have at least ONE abstract method!
		- if it has abstract method(s) they MUST be overridden
	- you CANNOT instantiate it
	- Used to include abstract methods or to prevent direct object creation.
	- It acts as an incomplete blueprint, with subclasses that inherit it being responsible for providing the concrete implementation.
- Abstract method
	- must be empty, the subclasses must extend the method
	- means => "This method has no body- u must override it in a subclass"
	- if a method is not abstract, it MUST have a body => default (method becomes concrete)
#### Example
- The Computer class itself is empty!
- Anyone can create an object of Computer, but we don't really want that. A Computer is just a *concept*
- So instead of actually defining the method, we can just declare it (remove the `{}`)

```java
abstract class Computer {
	// declaration of method
    public abstract void turnOn(); // no implementation — just a "what"
}

class Laptop extends Computer {
    public void turnOn() {
        System.out.println("Laptop turning on");
    }
}

class Desktop extends Computer {
    public void turnOn() {
        System.out.println("Desktop turning on");
    }
}
```

- You're defining a **"contract"** — all Computers **must implement `turnOn()`**
- The caller only needs to know it's a `Computer`, not the specific type
- ✅ This is **abstraction**.

- the subclass
	- 구현 클래스
	- MUST override the abstract methods it got from the super class (abstract class)
	- 그냥 상속받는거임, 다만 super이 abstract class일 뿐

```java
public void bootUp(Computer c) {
    c.turnOn();
}
```
- you don't care if it's a laptop, desktop, a server, anything is fine as long as it's a `Computer`!
### Rules
- If you have an abstract method in the class, the class should be abstract
- If you have an abstract class, you can have *both* abstract methods AND normal methods
- You CANNOT create objects of abstract classes => no instantiation allowed!
- Every class that `extends` the abstract class needs to implement ALL the abstract methods!

# Interface
- interface
	- methods - `public abstract`
		- ALL methods here are `public` and `abstract` by DEFAULT, so just remove the keywords `public abstract`
		- you CANNOT have normal methods
	- variables - `public static final` 
		- `public static final` variables ONLY!
		- u dont want to change the variable values
		- u usually don't even have variables, u just have methods usually
	- Can't instantiate from interfaces
- Characteristics
	- multiple inheritance possible <<<< the main difference with abstraction
		- `public interface TestClass implements Ip1, Ip2`
		- You can make a **utility interface** that combines lots of interfaces
	- More fit for scalability more than abstract classes
- Just think it's an empty shell

```java
interface Computer
{
	void turnOn(); 
}
```

|                              | Abstract                                                     | Interface                                                                                    |
| ---------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| Purpose                      | To share common code and define a base class                 | To define a contract (a set of methods) for classes to implement                             |
| Keyword                      | `abstract class`                                             | `interface`                                                                                  |
| Methods                      | Can have **abstract** and **concrete** (implemented) methods | All methods are **abstract** by default (except `default` and `static` methods since Java 8) |
| Constructors                 | Can have constructors                                        | Cannot have constructors                                                                     |
| Access Modifiers for Methods | Can have any (`public`, `protected`, `private`)              | Methods are implicitly `public`                                                              |
| When to use                  | When classes share common behavior and code                  | When unrelated classes need to implement the same behavior (contract)                        |
| keyword                      | `extend`                                                     | `implements`                                                                                 |
| Multiple inheritance         | ❌A class can **extend only one** abstract class              | ✅ A class can **implement multiple** interfaces (u have to define methods for all of them)   |
| Variables                    | Variables                                                    | Only constants (`public static final`) allowed. Access with the interface name itself        |
- Usually use interfaces for refactoring functions, abstract classes for refactoring fields