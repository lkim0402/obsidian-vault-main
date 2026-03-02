
>[!Stream]
>Stream is a sequence of elements that supports functional-style operations to process data in a declarative way.

- When you want to use streams, **you need to convert your data into a stream first.**
- Streams are good for **processing large amounts of data efficiently.**
    - When the data size is huge, streams can be faster than traditional loops.

- 중간연산자
	- return type이 stream이니까 계속 사용할 수 있는거임

# Stream characteristics
1. **A stream pipeline has three stages:**
    - Creation (stream source)
    - Intermediate operations (like filtering, mapping)
    - Terminal operation (like collecting, forEach)
2. **Streams do not modify the original data source; they are read-only.**
3. **Streams can be used only once. After a terminal operation, the stream is closed.**
4. **Streams use internal iteration (the stream controls the iteration, not you).**

# Example
## Using Array
`Arrays.stream()`
```java
import java.util.Arrays;
import java.util.stream.Stream;

public class StreamCreator {
    public static void main(String[] args) {
        String[] arr = {"김코딩", "이자바", "박해커"};
        Stream<String> stream = Arrays.stream(arr);
        stream.forEach(System.out::println);
    }
}
```

`Stream.of()`
```java
import java.util.stream.Stream;

public class StreamCreator {
    public static void main(String[] args) {
        String[] arr = {"김코딩", "이자바", "박해커"};
        Stream<String> stream = Stream.of(arr);
        stream.forEach(System.out::println);
    }
}
```
## Using Collection (List/set/etc)
- If you're using a [[Java Collections Framework (JCF)|collection]] (list, set, etc), you can use `stream()` to create + start a stream
```java
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

        // Use Stream to filter and print names starting with 'A'
        names.stream()
             .filter(name -> name.startsWith("A"))
             .forEach(System.out::println);
    }
}

```

# Operations
## Lazy evaluation
- intermediate operations in a Stream (like filter, map, etc.) are not executed immediately
	- they are recorded but not executed -> the terminal operations trigger the actual execution of the stream pipeline
- each element flows thorugh the entire pipeline (filter -> map -> etc) one by one, not stage by stage
- benefits
	- Performance optimization: unnecessary computations are avoided
## Intermediate operations (중간 연산)
- `filter, map` -> most used
- `sorted` 
- `distinct` -> removes duplicates
- `flatMap` -> flattens 
- `peek` -> used for debugging (doesn't touch the actual data)
- `skip, limit` -> skips `n` elements, leaves out `n` elements

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamCommonMethodsExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve", "Frank");

        List<String> processedNames = names.stream()
            .filter(name -> name.length() > 3)
            .map(String::toUpperCase)
            .sorted()
            .collect(Collectors.toList());

        processedNames.forEach(System.out::println);
    }
}

```
## Terminal operations (최종 연산)
- `forEach`
- `collect`
	- `Collectors.` -> `toList, toSet, toMap, joining`
		- `joining` -> just like [[JavaScript]]'s join method
- `allMatch, anyMatch, noneMatch`
	- returns bool
- `findFirst`, `findAny`
	- `findFirst` -> 순서대로 첫번째 여소 반환
	- `findAny` -> 병렬 처리 시 빠른 요소 반환
- `toArray`
	- converts to stream
# Anonymous functions VS lambda
- [notion 참고](https://www.notion.so/201096ba270c81b1815ed8f510918e9b)
- A **lambda** is just a **shorter (shorthand) way** to write an anonymous class that implements a **functional interface** (an interface with only one abstract method).
	- basically syntactic sugar!!
#### Anon
```java
public class FilterExample {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Anna");

        names.stream()
            .filter(new Predicate<String>() {
                @Override
                public boolean test(String name) {
                    return name.startsWith("A");
                }
            })
            .forEach(System.out::println);
    }
}
```
- The anonymous class creates a one-time implementation of `Predicate<String>`
	- `Predicate<String>` (`Predicate<T>`) is a [[Functional Interfaces & Descriptor|functional interface]], which means it has exactly 1 abstract method `test()`
	- it also only inputs `String`
- `filter()` accepts that object and calls its `test()` method on each stream element
#### lambda
```java
public class FilterExample {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Anna");

        names.stream()
            .filter(name -> name.startsWith("A"))
            .forEach(System.out::println);
    }
}
```