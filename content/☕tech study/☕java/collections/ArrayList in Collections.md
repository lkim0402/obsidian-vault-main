
>[!Arraylist]
>ArrayList in Java is a fundamental and widely used data structure within the Java Collections Framework. It's essentially a resizable array implementation of the List

- for [[Arrays in Java|normal arrays]], you have to decide the length of the array first (& cannot change it later)
- add, get, remove, clear
- Internally, it's actually an [[Arrays in Java|array]]!
	- It starts with default size 10
	- There is a pointer that indicates how much of the array is occupied
	- If increasing the arraylist, we're just adding the default size to the arraylist

```java
import java.util.ArrayList;  
  
public class Main {  
    public static void main(String[] args){  
        ArrayList nums = new ArrayList();  
        nums.add(true);  
        nums.add(4);  
        nums.add("hello");  
        System.out.println(nums);  //[true, 4, hello]
    }  
}
```
- **Raw type `ArrayList`**:
	- This means `nums` is a **raw type**, not `ArrayList<String>` or `ArrayList<Integer>`.
	- In this case, the compiler treats all elements as **`Object`** types (the root class of everything in Java).
- it just prints all the values!
	- the `ArrayList` **overrides** the `toString()` method from `Object`
# Basics
```java
ArrayList<IntegeR> list = new ArrayList<>();
list.add(1);
list.add(4);
list.add(3); // [1,4,3]

// getting item (index)
System.out.println(list.get(1)); 

// removing item at the last
list.remove(list.size() - 1); 

System.out.println(list.isEmpty()); // false

// sorting
Collections.sort(list); // [1,3,4]
```
- `list.get(1)`
	- getting item using index
- `list.remove(1)`
	- removing item using index
- `list.isEmpty()`
	- checks if empty
- `list.size()`
	- checks size
- `Collections.soft(list)`
	- sorts the list
## Initializing with another collection
```java
// you can initialize with another collection!
ArrayList<Integer> list2 = new ArrayList<>(list);
System.out.println(list2);
```

# The efficiency of ArrayLists


# Generics
- E - Element (used extensively by the Java Collections Framework, for example ArrayList, Set etc.)
- K - Key (Used in Map)
- N - Number
- T - Type
- V - Value (Used in Map)
- S,U,V etc. - 2nd, 3rd, 4th types

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
```
- `ArrayList` is of type `E`
	- `E` can be anything - object, integers (class representations of the primitive type), any custom class
- It extends `AbstractList<E>`
	- it is an [[Abstraction - Classes & Interfaces|abstract class]]
- it implements `List<E>`

It's always to specify the type
- Using generics you can specify the type you are working with
```java
import java.util.ArrayList;  
import java.util.List;  
  
public class Main {  
    public static void main(String[] args){  
        ArrayList<Integer> nums = new ArrayList<>();  
        // u can remove the type here  
        ArrayList<Integer> nums1 = new ArrayList<>(); 
        // u can use the interface/abstract class
        List<Integer> nums2 = new ArrayList<>();  
    }  
}
```

