# In variables
- In Java, constants (상수) are typically declared using the **final** keyword.
	- You can't change the value again (재할당이 불가능)

#### Final Variables (Constants)
- constants are usually uppercase
```java
final double PI = 3.141592;
// PI = 3.15 ->  error
```

#### Final Instance Variables (Object Fields)
You need to indicate that the variable is never null (used in spring)
```java
public class Test {
    final int number; // must be initialized in constructor

    public Test(int number) {
        this.number = number; // ✅ allowed: initialized once
    }
}
```

#### Final References (Objects)
- `final` prevents reassigning the reference, not modifying the object itself.
- You can still change fields or call methods on the object.

```java
final Person p1 = new Person("김신의");
p1.setName("문종모"); // ✅ allowed
System.out.println(p1.getName()); // 문종모

// p1 = new Person("다른 사람"); // ❌ Error: final reference cannot be reassigned
```

#### Practices in classes
- you can make an attribute `public final` and not private because you can't change it anyways even though it's public
- if it's public, you don't need a getter function anymore too
- best practice: Use `final` **and** `static` together for constants
```java
public class CodeitConstants {
    public static final double PI = 3.141592653589793;
    public static final double EULERS_NUMBER = 2.718281828459045;
    public static final String THIS_IS_HOW_TO_NAME_CONSTANT_VARIABLE = "Hello";

    public static void main(String[] args) {
        System.out.println(CodeitConstants.PI + CodeitConstants.EULERS_NUMBER);
    }
}
```

# In methods
- It will make the method non-overridable
```java
class A {
    public final void show() {
        System.out.println("A's show");
    }
}

class B extends A {
    // ❌ This will cause a compile-time error!
    public void show() {
        System.out.println("B's show");
    }
}

```
# In classes
- It will make the class non-inheritable
- When we want to stop overriding, we can use final methods
```java
public final class A
{
	public final void show()
	{
	}
}

// ❌ This will cause a compile-time error!
public final class B extends A ==> Error!
{
	public final void show()
	{
	}
}
```

