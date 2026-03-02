- [[Local VS Class VS Instance variables]]
# Instance variables
- Belong to **each individual object** (instance) of a class.
	- They are created when a class is instantiated with the `new`keyword
	- They last in the heap memory until the object's life cycle
- Automatically initialized with default values, depending on their data type
	- Reference type variables (참조자료형) have default `null` values
	- Primitive types have different default values based on their type
	- [[Primitive vs Reference types]]
- Basically they get overwritten when the constructor finishes if it assigns new values
- **Each object gets its own copy**
```java
public class Dog {
    String name;  // Instance variable

    public Dog(String name) {
        this.name = name;
    }
}

// Main.java
Dog dog1 = new Dog("Rex");
Dog dog2 = new Dog("Buddy");

System.out.println(dog1.name); // Rex
System.out.println(dog2.name); // Buddy

```
# Class variables
- Only created once when the class is loaded into memory
- Belong to the **class itself**, **not** any individual object.
	- This is why it's stored in the class area, and not the heap
- No need to create an instance to access this, u can use the class itself!
	- you *can* technically create an instance but it's not recommended
	- Shared by **all instances**
- uses `static` keyword 
	- [[Non-Access modifiers (기타 제어자)]]
```java
public class Dog {
    static int numberOfDogs = 0;  // Class variable

    public Dog() {
        numberOfDogs++;
    }
}

// Main.java
Dog dog1 = new Dog();
Dog dog2 = new Dog();

System.out.println(Dog.numberOfDogs); // 2
```
- just use it with the class itself (like `Dog.numberOfDogs`)
- With [[java/Garbage collection (GC)]]
	- Variables declared with static are not garbage collected for the entire runtime of the program because they belong to the class, not to individual instances
	- So, static variables live as long as the class is loaded, which often means for the entire duration the program runs
	- Because of this, you should carefully decide when to use static — unnecessary use can cause memory to be held longer than needed


