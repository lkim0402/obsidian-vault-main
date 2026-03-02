# Constructor
> Used when an instance of a class is created, initializes the attribute values
- if there is no constructor in the class, the compiler automatically makes a compiler that has no parameters
	- All classes have a constructor (whether explicitly made or not)
- roles
	- creates an instance
	- initializes the attribute values
- A constructor is called when using the `new` keyword
	- Java has 1 by default, you can explicitly create one
- Needs to be same name with the class

```java
public class Person 
{
	private String name;
	private int age;
	private int cashAmount;
	
	public Person(String pName, int pAge, int pCashAmount) 
	{
		name = pName;
		age = pAge;
		cashAmount = pCashAmount;
	}
	// constructors can also have method overloading
	public Person(String pName, int pAge) 
	{
		name = pName;
		age = pAge;
		cashAmount = 0;
	}
}

// can use it like this
Person p1 = new Person("Leejun", 23, 300000);
Person p1 = new Person("Kirby", 23);
```
- Constructor overloading is possible!
	- [[Polymorphism in java#Method Overloading|Method overloading]]

# `this`
> Refers to **the current object** — the instance of the class you're working inside
- 
#### Referring to the Current Object
- Distinguishes between instance variables and parameters
```java
public class Person {
    private String name;

    public Person(String name) {
        this.name = name; // 'this.name' refers to the instance variable
    }
}
```

#### Calling Another Constructor
- You can use `this(...)` to call **another constructor** in the **same class**
- Rules
	- Must be the **first statement** in the constructor.
	- Syntax: `this(arg1, arg2, ...)`
```java
public Person(String name) {
    this(name, 12, 0); // 12살을 기본 나이로 설정, 초기 현금 보유액은 0원.
}

public Person(String name, int age) {
    this(name, age, 0); // 초기 현금 보유액은 0원.
}

public Person(String name, int age, int cashAmount) {
    if (age < 0) {
        this.age = 12;
    } else {
        this.age = age;
    }

    if (cashAmount < 0) {
        this.cashAmount = 0;
    } else {
        this.cashAmount = cashAmount;
    }
    this.name = name;
}
```

- Inside the shorter constructors, we're calling the most complete constructor to reduce code duplication
- it's effective if the constructors with the least parameters call the constructor with the most parameters