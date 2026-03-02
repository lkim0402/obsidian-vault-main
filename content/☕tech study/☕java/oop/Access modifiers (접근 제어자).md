>Control who can access a class, field, method, or constructor

- can only use ONCE, unlike [[Non-Access modifiers (기타 제어자)|non-access modifiers]]
# Types
>The further, larger scope (public > protected > default > private)
- `public`
	- same package, same class
- `protected`
	- can be accessed by the subclass, regardless of the [[Java packaging|package]]
	- can be accessed by same package
	- Used a lot in studying design patterns
- `default` (no keyword)
- `private`
	- a private variable is only accessible through the current class

# Chart
>MEMORIZE THIS BRO

|                                | Public | Default | Protected | Private |
| ------------------------------ | ------ | ------- | --------- | ------- |
| Same class (the one we're in)  | ✅      | ✅       | ✅         | ✅       |
| Same package subclass          | ✅      | ✅       | ✅         | ❌       |
| Same package non-subclass      | ✅      | ✅       | ✅         | ❌       |
| Different package subclass     | ✅      | ❌       | ✅         | ❌       |
| Different package non-subclass | ✅      | ❌       | ❌         | ❌       |
- default is available in the same class & same package
	- subclass => `extends`
	- package => basically just folder
# Simple example
- Dangerous stuff can happen if you give too much access to an object, so you need to restrict its access
	- you can have a getter and setter to get & change your values
		- [[Encapsulation]]
```java
public class Person
{
	String name;
	private int age; // can only be accessed here

	public void setAge(int newAge)
	{
		if (newAge >= 0) // this method allows us to not set negative age
		{
			age = newAge;
		}
	}
	public int getAge()
	{
		return age;
	}
}
```

# Example 1
#### Same package 
```java
package package1;

public class Parent {
    private int a = 1;
    int b = 2;            // default
    protected int c = 3;
    public int d = 4;

    public void printAll() {
        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
        System.out.println(d);
    }
}

class Test {
    public static void main(String[] args) {
        Parent p = new Parent();
        // System.out.println(p.a); // 에러 발생: private 접근 불가
        System.out.println(p.b);    // default: 같은 패키지 접근 가능
        System.out.println(p.c);    // protected: 같은 패키지 접근 가능
        System.out.println(p.d);    // public: 어디서든 접근 가능
    }
}
```

#### Different package
```java
package package2;
import package1.Parent;

class Child extends Parent {
    public void print() {
        // System.out.println(a); // private: 접근 불가
        // System.out.println(b); // default: 접근 불가 (다른 패키지)
        System.out.println(c);    // protected: 상속 관계라 접근 가능
        System.out.println(d);    // public: 접근 가능
    }
}

public class Test2 {
    public static void main(String[] args) {
        Parent p = new Parent();
        // System.out.println(p.a); // private: 접근 불가
        // System.out.println(p.b); // default: 접근 불가
        // System.out.println(p.c); // protected: 같은 패키지+상속만 허용
        System.out.println(p.d);    // public: 접근 가능
    }
}
```