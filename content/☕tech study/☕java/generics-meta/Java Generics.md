
>[!Generics]
>Generics allow you to **parameterize types** (classes, interfaces, methods).

- Instead of specifying a type **upfront**, you **defer** that decision to when the class or method is used (compile-time, not runtime).
- Used extensively in [[Java Collections Framework (JCF)]] (e.g., `List<E>`, `Map<K,V>`).
- Cannot be used with **primitive types** directly → Use wrapper classes (`int → Integer`, `double → Double`)
- Cannot use with class-level variables
	- generics are **type parameters bound to instances**, and static members belong to the class itself, not any particular instance
	- If you could use generics to [[Class vs Instance variables#Class variables|class variables]], then **the type of the class variables would differ by instance**, making them **not share the same variable**
	- Therefore, there's **no type context** for the static field/method
### Common Type Parameter Names (convention):
- Although you *can* use lowercase names or other identifiers, [[☕Java]] convention prefers these uppercase letters for clarity.
- These are just names, and **the actual types are determined during runtime (when they are actually used)**

| Symbol | Meaning |
| ------ | ------- |
| `T`    | Type    |
| `E`    | Element |
| `K`    | Key     |
| `V`    | Value   |
| `N`    | Number  |
# Generic Class
- Classes where generics are being applied
- `<T>` is the type parameter
```java
class Basket<T> {
    private T item;

    public Basket(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }
}
```

- Since generic classes are not tied to a specific type, when instantiating a generic class, you need to specify the intended type like below:
```java
Basket<String>  basket1 = new Basket<String>("Hello");
Basket<Integer> basket2 = new Basket<Integer>(10);
Basket<Double>  basket3 = new Basket<Double>(3.14);

Basket<String> basket1 = new Basket<>("Hello"); // ✅ allowed
```
- **you cannot use primitive types** (like `int`, `double`, etc.) as type arguments -> use wrapper classes
- The compiler can **infer the type** from the variable declaration.
# Generic method
>[!generic method]
>While you can declare an entire class as generic, you can also declare **only specific methods** inside the class as generic. These are called **generic methods**.

- A generic method defines its **type parameter before the return type**, and this type parameter is only valid **within that method**.
	- 내부 구조 (static, heap, class method ) 이해 + 복습!
```java
class Basket<T> {
    private T item;

    public Basket(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }

	....
}
```
- The type parameter of a **generic method** is **completely separate** from the class’s generic type parameter — even if they use the same letter.
```java
class Basket<T> {
    public <T> void add(T element) {
        // This T is NOT the same as the class's T
    }
}
```
- **Class type parameter** is decided at **instantiation time**
- **Method type parameter** is decided at **method call time**
    
```java
Basket<String> basket = new Basket<>();
basket.<Integer>add(10); // 메서드 내 T = Integer
basket.add(10);          // 타입 추론 가능
```
- `basket.<Integer>add(10);`
	- the generic method `public <T> void add(T element)` is called with `T = Integer`
	- “For this call to `add`, treat the type parameter `<T>` as `Integer`.”
	- If the method was just `public void add(T element) { ... }`, we wouldn't be able to override the type
#### Printing stuff in list
```java
public class GenericPrinter {
	public static <T> void printArray(T[] array) {
		for (T elem: array) {
			System.out.println(element);
		}
	}
}
String[] names = {"Alice", "Bob", "Charlie"};
Integer[] numbers = {1, 2, 3};
GenericPrinter.printArray(names);
GenericPrinter.printArray(numbers);
```
#### Comparable
```java
public class GenericComparator {
    public static <T extends Comparable<T>> T max(T a, T b) {
        return a.compareTo(b) > 0 ? a : b;
    }
}

System.out.println(GenericComparator.max(10, 20));
System.out.println(GenericComparator.max("apple", "banana"));
```
- `public static <T extends Comparable<T>> T max(T a, T b)`
	-  `<T extends Comparable<T>>` : type parameter
		- `T` is a type that must implement `Comparable<T>`
		- Only **types that can compare themselves to other instances of the same type** (`T`) can be used here.
			- `Integer implements Comparable<Integer>`
			- `Double implements Comparable<Double>`
		- `T` should be something that can use the `.compareTo()` method
	- `T` : return type
		- same `T` declared in the type parameter
	- `max(T a, T b)`
		- Parameters: two values of type `T`
# Polymorphism
```java
class Flower { ... }
class Rose extends Flower { ... }
class RosePasta { ... }

class Basket<T> {
    private T item;

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }
}

class Main {
    public static void main(String[] args) {
        Basket<Flower> flowerBasket = new Basket<>();
        flowerBasket.setItem(new Rose());      // 다형성 적용
        flowerBasket.setItem(new RosePasta()); // 에러
    }
}
```
- You can also only allow subclasses that extend certain classes
# Bounded Type Parameter
- This helps ensure that only objects with a specific interface or superclass can be used, allowing access to the methods defined in the bound

```java
class Flower { ... }
class Rose extends Flower { ... }
class RosePasta { ... }

// HERE!!!
class Basket<T extends Flower> {
    private T item;
    public T getItem() { return item; }
    public void setItem(T item) { this.item = item; }
}

class Main {
    public static void main(String[] args) {
        Basket<Rose> roseBasket = new Basket<>();
        // Basket<RosePasta> rosePastaBasket = new Basket<>(); // 에러
    }
}
```
- Now `Basket` only allows subclasses of `Flower`
- You use `extends` keyword on both [[Abstraction - Classes & Interfaces|classes and interfaces]]
	- `extends` is used to set **upper bounds** on type parameters.  
	- `super` is **not** used in type parameter declarations like `<T super X>` — it's only used in wildcard types (e.g., `List<? super Integer>`).

```java
interface Plant { ... }
class Flower implements Plant { ... }
class Rose extends Flower implements Plant { ... }

class Basket<T extends Flower & Plant> {
    private T item;
    public T getItem() { return item; }
    public void setItem(T item) { this.item = item; }
}
```
- You use the `&` operator for both classes and/or interfaces
	- Classes must come first, the rest interfaces (otherwise it's a compile error)

# Wildcard
- used on the **consumer side** of generic objects, often in **method parameters**
- user when we **use the generic**
```java
public void addNumber(List<? super Integer> list) {
    list.add(123); // 가능
}
```
## Main concept
- **`extends`** = "this type or its subtypes" (upper bound/상한제한)
- **`super`** = "this type or its supertypes" (lower bound/하한제한)
- **PECS Rule**: **P**roducer **E**xtends, **C**onsumer **S**uper
    - Use `extends` when you're reading/getting values
    - Use `super` when you're writing/putting values
## Example
![[java-generic-example.png]]

| Function        | Description                                  | Constraint Syntax  | Translation                                              |
| --------------- | -------------------------------------------- | ------------------ | -------------------------------------------------------- |
| `call()`        | Available on all phones                      | `? extends Phone`  | Any type that IS `Phone` or EXTENDS FROM `Phone`         |
| `faceId()`      | Only available on iPhones                    | `? extends IPhone` | Any type that IS `IPhone` or EXTENDS FROM `IPhone`       |
| `samsungPay()`  | Only available on Samsung phones             | `? extends Galaxy` | Any type that IS `Galaxy` or EXTENDS FROM `Galaxy`       |
| `recordVoice()` | Only available on Android (excluding iPhone) | `? super Galaxy`   | Any type that IS `Galaxy` or is a SUPERCLASS OF `Galaxy` |
**1. `? extends` (Upper Bounded Wildcard)**
- Think of it as "Phone or below in the hierarchy"
- You can *READ* from these objects safely (because you know *they're at least a Phone*)
**2. `? super` (Lower Bounded Wildcard)**
- `? super Galaxy` -> Think of it as "Galaxy or above in the hierarchy"
- You can WRITE to these objects safely (because they can accept Galaxy objects)
**3. Why use `? super Galaxy` for `recordVoice()`?** The intention is to exclude iPhone family while allowing Galaxy family. 
- `? super Galaxy` accepts: `Galaxy`, `Phone`
- But it does NOT accept: `IPhone`, `IPhone12Pro`, `IPhoneXS`
- This effectively excludes the iPhone branch while allowing Android phones

#### PhoneFunction class with wildcard methods
```java
// User class that holds a phone
class User<T> {
    public T phone;
    
    public User(T phone) {
        this.phone = phone;
    }
}

class PhoneFunction {
    
    // ? extends Phone - accepts Phone and all its subclasses
    public static void call(User<? extends Phone> user) {
        System.out.println("📞 Calling with: " + user.phone.getClass().getSimpleName());
        // We can safely call Phone methods because all types extend Phone
        if (user.phone instanceof Phone) {
            ((Phone) user.phone).makeCall();
        }
    }
    
    // ? extends IPhone - accepts IPhone and all its subclasses
    public static void faceId(User<? extends IPhone> user) {
        System.out.println("🔒 Using Face ID with: " + user.phone.getClass().getSimpleName());
        // We can safely call IPhone methods because all types extend IPhone
        if (user.phone instanceof IPhone) {
            ((IPhone) user.phone).useFaceId();
        }
    }
    
    // ? extends Galaxy - accepts Galaxy and all its subclasses
    public static void samsungPay(User<? extends Galaxy> user) {
        System.out.println("💳 Using Samsung Pay with: " + user.phone.getClass().getSimpleName());
        // We can safely call Galaxy methods because all types extend Galaxy
        if (user.phone instanceof Galaxy) {
            ((Galaxy) user.phone).useSamsungPay();
        }
    }
    
    // ? super Galaxy - accepts Galaxy and all its superclasses (exclude iPhone family)
    public static void recordVoice(User<? super Galaxy> user) {
        System.out.println("🎤 Recording voice with: " + user.phone.getClass().getSimpleName());
        // This is trickier - we can't assume specific methods are available
        // because the type could be Phone (superclass) or Galaxy (exact class)
        System.out.println("Voice recording feature available for Android phones");
    }
}
```

#### Test cases
```java
// Test class
public class WildcardTest {
    public static void main(String[] args) {
        System.out.println("=== Testing call() - works with ALL phone types ===");
        PhoneFunction.call(new User<Phone>(new Phone()));
        PhoneFunction.call(new User<IPhone>(new IPhone()));
        PhoneFunction.call(new User<Galaxy>(new Galaxy()));
        PhoneFunction.call(new User<IPhone12Pro>(new IPhone12Pro()));
        PhoneFunction.call(new User<IPhoneXS>(new IPhoneXS()));
        PhoneFunction.call(new User<S22>(new S22()));
        PhoneFunction.call(new User<ZFlip3>(new ZFlip3()));
        
        System.out.println("\n=== Testing faceId() - works with iPhone family ONLY ===");
        // PhoneFunction.faceId(new User<Phone>(new Phone())); // ❌ Compile error!
        // PhoneFunction.faceId(new User<Galaxy>(new Galaxy())); // ❌ Compile error!
        PhoneFunction.faceId(new User<IPhone>(new IPhone())); // ✅ Works
        PhoneFunction.faceId(new User<IPhone12Pro>(new IPhone12Pro())); // ✅ Works
        PhoneFunction.faceId(new User<IPhoneXS>(new IPhoneXS())); // ✅ Works
        
        System.out.println("\n=== Testing samsungPay() - works with Galaxy family ONLY ===");
        // PhoneFunction.samsungPay(new User<Phone>(new Phone())); // ❌ Compile error!
        // PhoneFunction.samsungPay(new User<IPhone>(new IPhone())); // ❌ Compile error!
        PhoneFunction.samsungPay(new User<Galaxy>(new Galaxy())); // ✅ Works
        PhoneFunction.samsungPay(new User<S22>(new S22())); // ✅ Works
        PhoneFunction.samsungPay(new User<ZFlip3>(new ZFlip3())); // ✅ Works
        
        System.out.println("\n=== Testing recordVoice() - works with Galaxy and its supertypes ===");
        PhoneFunction.recordVoice(new User<Phone>(new Phone())); // ✅ Works
        PhoneFunction.recordVoice(new User<Galaxy>(new Galaxy())); // ✅ Works
        // PhoneFunction.recordVoice(new User<IPhone>(new IPhone())); // ❌ Compile error!
        // PhoneFunction.recordVoice(new User<S22>(new S22())); // ❌ Compile error!
    }

```


