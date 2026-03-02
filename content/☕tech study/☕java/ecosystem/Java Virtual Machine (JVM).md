
>[!jvm]
> **The JVM (Java Virtual Machine)** is a **virtual computer** designed to run Java applications.
>- It **reads and interprets `.class` files** (which are Java bytecode) and executes them.
>- It is the **core component** that allows Java to **run independently of the operating system**.
# How Java works
![[Pasted image 20250605102338.jpg]]
1. Write java source code (`.java`)
2. The java compiler (`javac`) compiles the source code into bytecode (`.class`)
3. The JVM specific to your OS executes the bytecode
	- As long as the **JVM for that platform** knows how to **interpret the bytecode**, your program will run the same everywhere

# How it works
- Regular Programs 
	- Programs request resources like CPU, memory, and input/output devices from the operating system
	- But since **each operating system handles these requests differently**, most programming languages are **dependent on the OS**
- How Java Works
	- Instead of interacting with the operating system directly, **Java uses the JVM to communicate indirectly** with the OS
	- Java Program → JVM → Operating System
	- Thanks to this, **Java code can run on any OS** without modification
- JVM has separate versions for different OS
	- Windows JVM, macOS JVM, Linux JVM, etc
- In the past
	- Ppl actually tuned the JVM (wtf?)
# Architecture
- https://www.notion.so/JVM-1fb096ba270c8191a256cdd0973533a7?pvs=4
	- actually just read this
- heap memory
	- all the objects are created here
		- `new` keyword objects,  like `new Person()` is in here
		- the object's address will be stored in the stack
- stack
	- Stores **method call frames** (also called stack frames)
	- Each **method call** creates a new frame on the stack => every method has its own stack
	- Stores:
		- **Local variables** (like method parameters)
		- **References** (addresses) to objects in the heap
- static methods are not stored in either heap or stack
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person(); // 👈 p is in the stack, new Person() is in the heap
    }
}
```