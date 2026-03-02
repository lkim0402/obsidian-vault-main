
>[!what]
>An immutable object in [[☕Java]] is an object whose state cannot be modified after it is created. 
>- Once an immutable object is initialized, its data cannot be changed. 
>- Any operation that appears to modify an immutable object will actually result in the creation of a new immutable object with the desired changes, leaving the original object untouched
>- When designing, immutability must be maintained by combining various elements such as [[Final|final]], private ([[Access modifiers (접근 제어자)|access modifiers]]), and defensive copies

- Examples
	- `String`, `Integer`, `LocalDate`
- Advantages
	- thread safe
		- no change even though it's accessed `n` times at the same time
	- Cacheability
		- Immutable objects can be easily cached because their state never changes

# Example
```java
public final class Person {

    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }
}
```
- Because there are no setter methods ([[Encapsulation|encapsulation]]), it's impossible to change the data externally
- This `Person` class is finalized when it is initialized
# Defensive Copies
```java
public final class Team {

    private final List<String> members;

    public Team(List<String> members) {
        this.members = new ArrayList<>(members); // 방어적 복사
    }

    public List<String> getMembers() {
        return new ArrayList<>(members); // 복사본 반환
    }
}

```
- When an internal field is a mutable object like `List<String>`, a defensive copy is essential.
- In the constructor, the input list must be copied, and the getter must also return a new copy to ensure safety from external modifications.

# String
- String itself is declared as `final` -> it's an immutable object!
- Methods that "Modify" Strings Return New Strings
	- When you perform operations that seem to modify a String, like concat(), substring(), replace(), toLowerCase(), etc., they actually return a new String object containing the result
```java
String s1 = "Hello"; // s1 refers to a String object "Hello" in the String Pool
String s2 = s1.concat(" World"); // s2 refers to a NEW String object "Hello World"

System.out.println(s1); // Output: Hello (s1 remains unchanged)
System.out.println(s2); // Output: Hello World
```

#### Example
```java
String test = "Hello";
test = "World";
```
- You are not modifying the original "Hello" String object

`String test = "Hello";`
- The variable `test` holds a **reference** to the "Hello" String object.
- If "Hello" already exists in the String Pool, test will reference that existing object. Otherwise, "Hello" is added to the pool, and test points to it.
```
Memory (simplified)
-----------------------
String Pool: ["Hello"]
-----------------------
test -> (reference to "Hello")
```

`test = "World";`
- A new String object with the value "World" is created. Again, if "World" already exists in the String Pool, test will reference that. Otherwise, "World" is added to the pool, and test points to it.
- The variable test is now reassigned to hold a reference to the new "World" String object. The original reference to "Hello" is lost from the test variable.
```
Memory (simplified)
-----------------------
String Pool: ["Hello", "World"]
-----------------------
test -> (reference to "World")
```
- **the "Hello" object itself remains unchanged in memory**
	- becomes part of [[Garbage collection (GC)]] lol
