
>[!what]
>A **method reference** is a shorthand syntax for a lambda expression that **calls an existing method**
>- Instead of writing the full lambda, you just refer directly to a method by name.
>- It creates an anonymous object that implements an functional interface

```java
// 람다식
(left, right) -> Math.max(left, right)

// 메서드 참조
Math::max
```

## Static Method Reference (정적 메서드 참조)
- `ClassName::staticMethod`
	- `(args) -> ClassName.staticMethod(args)`
- You reference the static method in the class
```java
import java.util.function.IntBinaryOperator;

public class Main {
    public static void main(String[] args) {
        IntBinaryOperator operator = Math::max;
        // Math::max is equal to (left, right) -> Math.max(left, right)
        System.out.println(operator.applyAsInt(3, 5)); // Output: 5
    }
}
```
- `IntBinaryOperator` is a functional interface that already exists in the standard library

## Bound Instance Method Reference
- `object::instanceMethod`
	- `(args) -> object.instanceMethod(args)`
- You reference the instance method from the instance
```java
Calculator calculator = new Calculator();
IntBinaryOperator operator = calculator::instanceMethod;
System.out.println(operator.applyAsInt(3, 5));
// 출력: 15
```

## Unbound Instance Method Reference (임의 객체의 인스턴스 메서드 참조)
- `ClassName::instanceMethod`
	- `(obj, args) -> obj.instanceMethod(args)`
- This syntax refers to an instance method that will be called on an unspecified (arbitrary) object of the specified class
	- Java will apply the method to the first argument, treating it as the instance
```java
List<String> list = List.of("APPLE", "BANANA", "CHERRY");
list.stream()
    .map(String::toLowerCase)  // equivalent to s -> s.toLowerCase()
    .forEach(System.out::println);
```
- `String::toLowerCase` means “call `.toLowerCase()` on each String in the stream”.
- The stream passes each element as the implicit object

```java
BiFunction<String, String, Boolean> biFunc = String::equals;
System.out.println(biFunc.apply("test", "test"));
// 출력: true
```

## Reference to a Constructor (생성자 참조)
- `ClassName::new`
	- `(args) -> new ClassName(args)`
```java
Supplier<List<String>> listSupplier = ArrayList::new;
// Equivalent to: () -> new ArrayList<String>()
```

# Examples
```java
public class Member {
  private String name;
  private String id;

  public Member() 
  { 
	  System.out.println("기본 생성자"); 
  }
  
  public Member(String id) 
  { 
	  System.out.println("id 생성자"); 
	  this.id = id; 
  }
  
  public Member(String name, String id) 
  {
    System.out.println("name, id 생성자");
    this.name = name; 
    this.id = id;
  }
}

public class ConstructorRef {
  public static void main(String[] args) {
    Function<String, Member> f1 = Member::new;
    Member m1 = f1.apply("kimcoding");

    BiFunction<String, String, Member> f2 = Member::new;
    Member m2 = f2.apply("kimcoding", "김코딩");
  }
}
```