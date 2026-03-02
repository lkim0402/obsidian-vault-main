 - [[Java's Memory Model]]
- [[Pass-by-value and Pass-by-Reference]]
# Primitive types 
>[!Overview]
>The simplest and most basic data types in Java that represent raw values such as numbers and characters.
- The variable contains the value itself (8 total)
	- `boolean`
	- `char` - should be `''`, used in `String` (collection of `char`)
	- `double` - the default for floating-point values
	- `int`
	- `float`
	- `byte`
	- `long`
	- `short`
- The fundamentals are `boolean`, `char`, `double`, and `int` (the rest are variations)
	- `int`: 4 bytes
	- `double`: 8 bytes
	- `char`: 2 bytes (for UTF-16 characters)
	- `boolean`: 1 bit
```java
// 변수 선언 variable declaration
int a; // type variableName;
```
- camelCase recommended, always start with lowercase

```java
// primitive types
int myInt = 123;
long myLong = 12343245325L; // add L (or l)
float myFloat = 3.14f; // add f
double myDouble = 3.143252;
boolean myBoolean = true;

char a1 = 'a';
char a2 = 97;

// class
String myString = "hello";
```
- String is actually a class
	- the `myString` variable is now the String class's instance

|Category|Type|Example|
|---|---|---|
|Integer types|`byte`, `short`, `int`, `long`|10, -5, 30000|
|Floating-point types|`float`, `double`|3.14f, 2.71828|
|Character type|`char`|'A'|
|Boolean type|`boolean`|true, false|
## Summary chart
- 📌= the fundamentals
- For all signed integer primitives, the range is:$−2^{(n−1)} \text{ to } 2^{(n−1)}−1$
	- `n` = number of bits
- `char` is unsigned, so it's range is `0` to `(2¹⁶ − 1)`
- Floating-point ranges are _approximate_ because of IEEE 754 representation — they store a **sign bit**, **exponent**, and **mantissa (fraction)**.

| **Type**    | **Category**              | **Size (Bytes)**                                        | **Bit Width** | **Range (in numbers)**                                   | **Range (in powers of 2)** | **Default Value** |
| ----------- | ------------------------- | ------------------------------------------------------- | ------------- | -------------------------------------------------------- | -------------------------- | ----------------- |
| `byte`      | Integer                   | 1                                                       | 8             | −128 to +127                                             | −2⁷ to (2⁷ − 1)            | `0`               |
| 📌`boolean` | Logical                   | JVM-dependent (1 bit nominally, often 1 byte in memory) | —             | `true` or `false`                                        |                            |                   |
| 📌`char`    | Character (Unicode)       | 2                                                       | 16            | 0 to 65,535                                              | 0 to (2¹⁶ − 1)             | `'\u0000'`        |
| `short`     | Integer                   | 2                                                       | 16            | −32,768 to +32,767                                       | −2¹⁵ to (2¹⁵ − 1)          | `0`               |
| 📌`int`     | Integer                   | 4                                                       | 32            | −2,147,483,648 to +2,147,483,647                         | −2³¹ to (2³¹ − 1)          | `0`               |
| `float`     | Floating-point (IEEE 754) | 4                                                       | 32            | ≈ ±3.40282347×10³⁸                                       | ±(2⁻¹²⁶ to 2¹²⁸)           | `0.0f`            |
| `long`      | Integer                   | 8                                                       | 64            | −9,223,372,036,854,775,808 to +9,223,372,036,854,775,807 | −2⁶³ to (2⁶³ − 1)          | `0L`              |
| 📌`double`  | Floating-point (IEEE 754) | 8                                                       | 64            | ≈ ±1.79769313486231570×10³⁰⁸                             | ±(2⁻¹⁰²² to 2¹⁰²³)         | `0.0d`            |
- `byte`
	- from `00000000` to `11111111`
	- the *leftmost bit*, or the *most significant bit (MSB)* is the sign bit (`0` = +, `1` = -)
		- The largest positive value is `01111111`, which is $2^6 + 2^5 + ...2^1 + 2^0 = 127$
		- The smallest negative value is `10000000`, which is -128
## About the numbers
- Whole numbers (Integers)
	- `short` 
		- "short" integer, range is from (-32,768 to 32,767)
		- Rarely used in modern day programming, used in very memory-sensitive environments
	- `int` (Integer)
		- Range is from roughly -2 billion to +2 billion
		- largest `int` is $2^{31}$ - 1, which is  `01111111111111111111111111111111` (the `0` is the MSB)
		- This is normally used
	- `long` 
		- "long" integer, add `L` in the end
		- Use for numbers way bigger than 2 billion
		- Use for massive numbers, like scientific calculations, user IDs in a huge [[🗃️Database|database]]
- Decimals (with fractional parts)
	- `double`
		- The most common data type for decimals => In java, it's the standard and recommended choice
		- "double" for "double precision", accurate up to about 15 decimal digits
		- Use for any calculation involving decimals
	- `float`
		- A "floating-point" number with less precision than a `double`. It's only accurate to about 6-7 decimal digits
		- Not that used, similar to `short` it's used in memory-sensitive settings
## Two's Complement
- Purpose: Represent signed integers in binary, allowing:
    1. Simple arithmetic (addition/subtraction)
    2. One unique representation of zero
    3. A symmetric, consistent range of negatives
- Steps
	1. Invert all bits (flip 0 ↔ 1)
	2. Add 1
	3. Make the result negative
- Example
	- `11111111` → invert → `00000000` → add 1 → `00000001` → represents `−1`
	- `11111110` → invert → `00000001` → add 1 → `00000010` → represents `−2`
	- `00000101` (+5) → invert → `11111010` → add 1 → `11111011`
- So for the smallest negative value:
	- `10000000` → invert → `01111111` → add 1 → `10000000` → represents `-128`
# Reference type 
- The variable points to where the value is stored
	- Instead of storing data directly, they store an `8-byte identifier`, which refers to the location of an object stored in the **heap**, a special memory area managed by [[Java Virtual Machine (JVM)]] for dynamic memory
- 📌A dual storage structure
	- The reference variable (the `8 byte identifier`), which is stored either in the *stack* if it's a local variable, or the *heap* if it's an [[Class vs Instance variables#Instance variables|instance variable]]
	- The object itself stored in the heap, where the JVM manages
- `null` can only be stored in reference types!
	- the default value of a reference type is `null`
- types
	- objects
	- String
	- `int[]`
```java
Person p1, p2;
p1 = new Person("Leejun", 23);
p2 = p1;
p2.setName("Kirby") // p1 name's will change too!
```
- By convention, the class starts with a capital letter
- `p1` and `p2` *points* to the same person instance
## Reference types are NOT pointers!

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*HcLmniiV55cpIRIxrzJZdQ.png)

```java
Person p = new Person();
```
- `p` is a reference variable that holds a *reference* to an object in the heap
	- A *reference* = a safe, opaque pointer that the [[Java Virtual Machine (JVM)]] controls.
	- Basically acts like a pointer but without the power/danger lol, it's restricted to preserve memory safety and to avoid undefined behavior
- Unlike in C or C++:
	- You **cannot** access or manipulate the actual memory address.
	- You **cannot** do pointer arithmetic (`p + 1`, `&p`, `*p`, etc.).
	- You **cannot** explicitly free memory (`delete` or `free`).
	- You **cannot** take the address of a variable.

|Aspect|C/C++ Pointer|Java Reference|
|---|---|---|
|What it stores|Actual memory address|Abstract reference (JVM-managed handle)|
|Can you get the address?|Yes (`&variable`)|No|
|Can you do arithmetic?|Yes (`ptr++`)|No|
|Can you dereference manually?|Yes (`*ptr`)|No — automatic dereferencing via `.`|
|Can you `free()` it manually?|Yes|No (garbage collector handles it)|
|Can it point to stack memory?|Yes|No (only heap objects)|
|Null possible?|Yes|Yes (`null` reference)|
- Also Java references are **typed** -> the compiler and runtime know exactly what class or array type they refer to
	- You can't mix up types without explicit casting, which prevents the kind of undefined memory access that C/C++ pointers can cause
	- Java also uses garbage collection, so the reference doesn't have to be manually freed: when no reference points to an object, the [[Java Virtual Machine (JVM)|JVM]] automatically reclaims it