
>[!Overview]
>Java uses **pass-by-value** exclusively, even for reference types!
>- Pass by value: The method parameter values are copied to another variable and then the copied object is passed to the method. The method uses the copy.
>- Pass by reference: An alias or reference to the actual parameter is passed to the method. The method accesses the actual parameter.

- [[Primitive vs Reference types]]
# Scenarios
- When passing arguments to method's parameters, it's one of the 2:
	- **Primitives**: A *copy* of the primitive value is passed
	- **Reference types**: A copy of the 8-byte reference is passed, meaning the method receives a reference to the same object
- Takeaways
	- Reassigning primitives doesn’t affect the caller’s variable.
	- Reassigning reference parameters doesn’t affect the caller’s variable.
		- Modifying the object _through_ the reference _does_ affect it, because both variables refer to the same object.
# Examples
## With primitives
```java
public class Main {
    public static void main(String[] args) {
        int x = 5;
        changeValue(x);
        System.out.println(x); // ?
    }

    static void changeValue(int n) {
        n = 10;
    }
}
```
- `x` in `main` = 5 (stack)
- When calling `changeValue(x)`, the _value 5_ is **copied** into `n`.
- Inside `changeValue`, `n = 10` modifies the local copy — not the original.
- So it prints `5`
## With reference types
```java
class Person {
    String name;
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.name = "Alice";
        changeName(p);
        System.out.println(p.name); // ?
    }

    static void changeName(Person person) {
        person.name = "Bob";
    }
}
```
- when calling `changeName(p)`,  a *copy of the reference* is passed
- `person.name`
	- then `p` and `person` both point to the same heap object, so this actually changes the name of the person object
- So it prints `Bob`
### What if you reassign the reference inside the method?
```java
static void changeName(Person person) {
    person = new Person(); // reassign reference
    person.name = "Charlie";
}
```
- `person = new Person();`
	- `person` now points to a different object (`"Charlie"`)
	- this only affects the local copy of the reference, so the original `p` in `main` still points to `Alice`
- Now it would print `Alice`
# Sources
- [DigitalOcean guide](https://www.digitalocean.com/community/tutorials/java-is-pass-by-value-and-not-pass-by-reference)