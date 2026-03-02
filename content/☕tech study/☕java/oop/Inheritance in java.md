
>[!key]
>A mechanism where one class inherits fields and methods from another class.
>- Use inheritance when you need hierarchy among ur objects

- Everything that is not private in the original class will show up in the new class 
	- `extends` keyword 
	- the new class ALWAYS have the same or more members than the super class
- There are no multiple inheritance of classes in java (multiple parents)
- scalability

```java
// Superclass / parent class
public class Keyboard {

    int keys = 100;
    String color = "White";

    public void pressed()
    {
        System.out.println("Pressed the keyboard");
    }

    public void throwIt()
    {
        System.out.println("Got hit!");
    }
}

// Subclass / child class
class AdvKeyboard extends Keyboard
{
     public void hitNum()
     {
         System.out.println("Hit the keyboard");
     }
}
```

- Used commonly to represent hierarchical structures (계층 구조를 위해 많이 쓰임)
	- Person > Programmer > Backend, Frontend, AI, etc
- Superclass
	- the parent => super class is the official term
- Subclass
	- If we don't write any constructors, it has the default constructor which contains `super()`
		- so the constructors are not inherited, but just called through `super()`
- [[Polymorphism in java#Method Overriding (재정의)|Method Overriding (재정의)]]

# type casting
```java
Person p1 = new Programmer();
```
-  the type actually automatically changes to Person from Programmer
- but you wont be able to access the fields/methods only in programmer
- [[Polymorphism in java#`instanceof`|using instanceof]]
# super
- kind of similar to [[Constructor & this#`this`|this keyword]]
- you can use this if and only if the classes tied to [[Inheritance in java|inheritance]]
#### Referencing members
- Can reference members of the super class
	- Useful if you have members with the same name with the sub & super class
```java
public class Example {
    public static void main(String[] args) {
        SubClass sub = new SubClass();
        sub.callNum();
    }
}

class SuperClass {
    int count = 20; // super.count
}

class SubClass extends SuperClass {
    int count = 15; // this.count

    void callNum() {
        System.out.println("count = " + count);           // 15
        System.out.println("this.count = " + this.count); // 15
        System.out.println("super.count = " + super.count); // 20
    }
}
```

#### Calling constructor
- You only put it at the first line
```java
Class Person 
{
	String name;
	int age;

	public Person(String name, int age)
	{
		this.name = name;
		this.age = age;
	}

	void walk()
	{
		System.out.println("Walking");
	}
}

class Programmer extends Person
{
	public Programmer(String name, int age)
	{
		super(name, age);
	}
	public Programmer(String name, int age, String companyName)
	{
		this(name, age);
		this.companyName = companyName;
	}
}
```
- Just like [[Constructor & this#`this`|this()]], you need to put it at the 1st line of ur constructor 
- It calls the constructor of your super class with the arguments you put in
# Disadvantages
- tight coupling
	- if the super class changes, then the subsequent subclass will also chang
	- 남용하면 오히려 유지보수가 어려울 수 있음
- Hard to keep track with deeper layers
	- java의 packages들은 상속이 엄청 많이 되어있음
	- 상속을 공부하려면 java의 package들을 보자 << 생각보다 엄청 도움 됨
# Object
- all classes inherit from `Object` automatically
	- and they can use `Object`'s methods