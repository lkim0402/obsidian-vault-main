
>[!What]
>A functional descriptor is just the abstract method signature of a functional interface.
>- Expresses what data are the parameters and what is the return type
>	- 단어 그대로 함수가 어떤 입력값을 받고 어떤 반환값을 주는지에 대한 설명을 람다 표현식 문법으로 표현한 것
>- A functional interface is any interface in Java that has exactly **one abstract method**

| Functional Interface | Functional Descriptor (추상 메서드가 어떤 역할을 하는지 간략하게 설명) |
| -------------------- | -------------------------------------------------- |
| Predicate            | T -> boolean                                       |
| Consumer             | T -> void                                          |
| Function<T,R>        | T -> R                                             |
| Supplier             | ( ) -> T                                           |
| BiPredicate<L, R>    | (L, R) -> boolean                                  |
| BiConsumer<L, R>     | (T, U) -> void                                     |
| BiFunction<T,U,R>    | (T, U) -> R                                        |
| Runnable             | () -> void                                         |
- 출처: [https://inpa.tistory.com/entry/🚀-Function-Descriptor](https://inpa.tistory.com/entry/%F0%9F%9A%80-Function-Descriptor) [Inpa Dev 👨‍💻:티스토리]
- [Function Descriptor](https://www.notion.so/Function-Descriptor-201096ba270c81e2824af159846908ec?pvs=4)
- using generics
	- generics을 잘 쓴 코드는 품질이 높고, 나중에 좋은 회사 갈지 말지 판단하는 기준이 됨
- Use if you want to force the type you want to use
- 기본적으로 만들어진 애들은 편의로 만들어 놓은거임
	- 우리가 똑같은거 또 만들어도 됨
****

## Example
- They are both: `(T, U) -> R`, which can be implemented by `BiFunction<T,U,R>`
```java
@FunctionalInterface
public interface Calculator {
    int compute(int a, int b); 
    // must match Math.max(int, int)
}

// same thing with above except written differently
@FunctionalInterface
public interface Calculator<T,U,R>
{
	R calculate(T a, U b);
}

```

## Rules
- A lambda expression must match the functional interface's descriptor — which means it must implement the interface's abstract method.
- To assign a lambda to a functional interface, the method signature (i.e. parameter types and return type) must match exactly.
## With method reference
- [[Method Reference]]
```java
import java.util.function.BiFunction;

public class Test {
    public static void main(String[] args) {
        // Using custom functional interface
        Calculator c = Math::max;
        System.out.println("Custom Calculator: " + c.compute(10, 20));

        // Using built-in BiFunction
        BiFunction<Integer, Integer, Integer> f = Math::max;
        System.out.println("Built-in BiFunction: " + f.apply(10, 20));
    }
}
```
- With constructors
	- `Function<String,Member>`로 자동 유추 가능함
	- Image![[functional_descriptor.png]]


