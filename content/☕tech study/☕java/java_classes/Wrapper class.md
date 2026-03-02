
>[!Wrapper class]
>An object representation of a primitive type (like int, double, char, etc.)
>- Used when u need to use primitive types as if they were reference types
>- 💡 The term "wrapper" signifies that it "wraps" a primitive value into an object form.

- [[☕Java]] is an object-oriented language, but primitive types like int, double, etc. are not objects. 
	- [[Primitive vs Reference types]]
- Sometimes you need an *object version of these* to work with features like:
	- [[Java Collections Framework (JCF)]] (e.g., `ArrayList<Integer>`, not `ArrayList<int>`)
	- [[Java Generics|Generics]]
	- `Null` values
	- Methods that require objects
- ***All primitive types has a wrapper class***

| Primitive Class | Wrapper class (래퍼 클래스) |
| --------------- | ---------------------- |
| `byte`          | `Byte`                 |
| `short`         | `Short`                |
| `int`           | `Integer`              |
| `long`          | `Long`                 |
| `float`         | `Float`                |
| `double`        | `Double`               |
| `char`          | `Character`            |
| `boolean`       | `Boolean`              |

Wrapper class instances can be made with [[Constructor & this|constructors]] or literals
```java
Integer i = Integer.valueOf(123);
Integer i = new Integer(123); // constructor
Integer i = 123; // literals

System.out.println(123 == 123); // true
System.out.println(new Integer(123) == new Integer(123)); // false
// they both point to different instances -> have to use .equals
System.out.println(new Integer(123).equals(new Integer(123)));
```
- Many built-in classes provide such methods for performance reasons, and their use is recommended, if not compulsory. 
	- In the case of `Integer`, you could still create an object with the `new` keyword, but you will get a deprecation warning.
# Boxing and Unboxing
![[boxing_unboxing_java.png]]
#### Boxing and Unboxing
- **Boxing**: Converting a **primitive type** into its corresponding **Wrapper Class object**.
- **Unboxing**: Converting a **Wrapper Class object** back into its **primitive type**.
- **AutoBoxing** / **AutoUnboxing**
	- Since JDK 1.5, the compiler automatically handles these conversions for you

#### Comparing Values & Caching
```java
Integer num1 = 100; // Autoboxed
Integer num2 = 100; // Autoboxed
Integer num3 = 200; // Autoboxed
Integer num4 = 200; // Autoboxed

System.out.println(num1 == num2); // True for small values (due to Integer caching)
System.out.println(num3 == num4); // False for larger values (new objects are created)

System.out.println(num1.equals(num2)); // True (compares values)
System.out.println(num3.equals(num4)); // True (compares values)
```
- For `Integer` objects with values between -128 and 127 (inclusive), Java often **caches** these objects
	- if you declare `Integer num1 = 100;` and `Integer num2 = 100;`, `num1` and `num2` might actually refer to the _same_ object in memory, leading `==` to return `true`
- However, for values outside this range, new objects are typically created, and `==` would return `false` even if the values are the same
- **Always use `equals()` to compare the values of Wrapper Class objects to avoid unexpected behavior!**

|Class|Caching Range|Is Caching Performed?|
|---|---|---|
|Byte|All Values|✔|
|Short|-128 ~ 127|✔|
|Integer|-128 ~ 127|✔|
|Long|-128 ~ 127|✔|
|Character|0 ~ 127 (ASCII)|✔|
|Boolean|true / false|✔|
|Float|None|❌|
|Double|None|❌|

# With Collections
- Java [[Java Collections Framework (JCF)]] can only use reference types. Since primitive types cannot be stored directly, wrapper classes are utilized to ensure the type safety of [[Java Generics|generics]].

```java
class StringListWrapper {
    private List<String> list = new ArrayList<>();

    public void add(String value) {
        list.add(value);
    }

    public String get(int index) {
        return list.get(index);
    }
}
```