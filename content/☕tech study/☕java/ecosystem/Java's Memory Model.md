# Stack
- The stack memory holds **primitive values** and **references** (the `8 byte identifiers`)
	- `int`: 4 bytes
	- `double`: 8 bytes
	- `char`: 2 bytes
	- `Reference`: 8 bytes (identifier)
	- etc…
- Each thread has its own stack; it’s fast and automatically managed (LIFO).
# Heap
- This area holds **complex objects** such as arrays, strings, and instances of classes
	- All actual **objects** and **arrays** created with `new`
	- Also stores all the variables in the objects. 
		- So if object `Person p` has fields `int age`, then the reference to the object `p` is in the stack, but the object itself and its fields (`int age`) are in the heap
- Shared by all threads; garbage-collected when objects are no longer referenced.
- 📌String pool
	- String literals are stored here!
# Method area
- the part of the [[Java Virtual Machine (JVM)]] where class metadata and static members live
# Cases
## Local variables
- 📌Local primitive -> stack 
- 📌Local reference -> identifier in stack, the actual object in heap
```java
public class Main {
    public static void main (String[] args) {
	    Person p = new Person();  // p is local
	    int x = 5;
	}
}
```
- `Person p = new Person();`
    - `p`: local reference variable → stored in stack frame of `main()`
    - `new Person()`: actual object → stored in heap
    - When `main()` returns, stack frame is popped — `p` disappears, object remains in heap until GC
- `int x = 5;`
    - `x`: local primitive → stored directly in stack frame
    - Value `5` stored right there in stack (no heap allocation)
    - When `main()` returns, `x` disappears
##  Instance variables (fields of object)
- When an object is allocated on the heap, space for all its fields (both primitives and references) is reserved in that heap block!
📌Primitive instance variables -> heap (as part of the object stored in heap)
📌Reference instance variables -> both the reference (as part of object) and the object itself stored in heap
```java
class Team {
    Player leader; // reference instance variable
    int totalPlayers; // primitive instance variable
}
public class Main {
	public static void main(String[] args) {
		Team t = new Team();
		
		t.leader = new Player();
		t.totalPlayers = 10;
	}
}
```
- `Team t = new Team();` 
	- `t`: a local variable in the stack that points to new Team object 
	- `new Team();`: the instantiated team object that lives in the heap
- `t.leader = new Player();`
	- `leader`: reference variable inside the Team object (heap) that points to another `Player` object (also in the heap)
	- `new Player()`: another object in heap
- `t.totalPlayers = 10;`
	- `totalPlayers`: primitive field stored directly inside the Team object (heap) as part of the object's memory layout)
## Static/Class variables (fields of class)
📌 Static reference variable → stored in method area (Metaspace), object in heap  
📌 Static primitive variable → stored in method area (Metaspace)
```java
class Team {
    static Coach headCoach = new Coach();
    static int teamPlayer = 0;
}
```
- `headCoach`: static variable stored in method area (Metaspace)
    - Points to a `Coach` object in the heap
- `teamPlayer`: static primitive variable stored in method area (Metaspace), value `0` stored directly there
# Example
```java
class DataHolder {
    // Primitive Instance variable, It will be stored in the object in the heap when object is instantiated)
    int primitiveValue;          
    // Reference Instance variable, It will be stored in the object in the heap when object is instantiated), pointing to a String in the heap.
    String referenceValue;       
}

public class Main {
    public static void main(String[] args) {
        // stored in the stack (primitive type local variable)
        int primitive = 10;      
        // 'holder' reference stored in stack pointing to a DataHolder object in heap
        DataHolder holder = new DataHolder(); 
        // primitive instance variable stored in the object in heap
        holder.primitiveValue = 42;  
        // Reference Instance variable Points to a String object in heap
        holder.referenceValue = "Hello World"; 
    }
}
```
- `holder` is an 8 byte reference stored in the stack, referring to a `DataHolder` object in the heap
- The `primitiveValue` is stored within the `DataHolder` object in the heap.
- The `referenceValue` holds an 8-byte identifier that points to a `String` object, also in the heap.

![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*YX_W-mMnw9g7UMcfyBWraA.png)