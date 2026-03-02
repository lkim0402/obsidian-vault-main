
>[!Overview]
>Represents a combination of characters that make up a string
>- String is a class so when we make a new string we're actually creating an [[OOP Intro#Classes/Objects|object]]
>- You can create String objects using string literals or the `new` keyword
- String is immutable! -> Use `StringBuffer` (or the more modern `StringBuilder`) when you need a **mutable** string
- You MUST use `"` and not `'`, since `'` is reserved for chars
# Basics
```java
// same thing
String myString = new String("aBc"); // new keyword 
String myString = "aBc"; // String literal

// returns new string
System.out.println(myString.toUpperCase()) // ABC
System.out.println(myString.toLowerCase()) // abc
System.out.println(myString)               // aBc

// string concatenation
System.out.println(a + "+" + b " = " + (a + b));
System.out.println("데카르트는 \"나는 생각한다. 고로 존재한다.\"라고 말했다.");
```
- `String myString = new String("aBc");`
	- `myString` reference is stored in the stack, and the String object itself is stored in the heap
	- [[Java's Memory Model]]
- string concatenation
	- Combining several strings into a single string, or combining a string with other data into a new, longer string.
	- often used to report the value of a variable
- `print` VS `println`
	- `println`: sends output to current line then moves to the beginning of a new line
	- `print`: only sends output to current line
		- so it doesn't go to the beginning of a new line
# String Literal
```java
String s1 = "hello";
String s2 = "hello";
```
- `s1` and `s2` are references pointing to `String` objects.
- All string literals in a special area of memory called the **String Constant Pool**.
- **How it works:** When you create `s1`, Java puts the object `"hello"` into the pool. When you create `s2`, Java checks the pool, sees that `"hello"` already exists, and simply makes `s2` point to that _exact same object_.
	- They point to the same place in memory!
# `String` class: `new` keyword
```java
String s3 = new String("hello");
String s4 = new String("hello");
```
- Explicitly creates a new object every time!
- The `new` keyword forces Java to create a brand new `String` object in the **main memory area (the heap)**, *completely separate* from the String Constant Pool.
- **How it works:** Even though `s3` and `s4` contain the exact same characters, they are two distinct objects in different memory locations.
# Escape sequences
- two-character sequences used to represent special characters, all beginning with `\`
```java
System.out.println("What \"characters\" does this \\ print?");
// Output: What "characters" does this \ print?
```
- Use `\n` for line break
# StringBuffer
```java
StringBuffer s = new StringBuffer("hello");
s.append("!"); //s = "hello!"
```

# Comparison
```java
// comparing
String myString = "aBc";
System.out.println(myString.toLowerCase() == "abc"); // comparing instances (addresses)
System.out.println(myString.toLowerCase().equals("abc")); // comparing values
```
- If you compare 2 primitives types, you compare the value
- If you compare 2 reference types, you compare if they point to the same instances