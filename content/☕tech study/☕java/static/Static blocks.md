
>[!definition]
>A **static block** is a block of code enclosed in `{}` and marked with the `static` keyword. 
>- It runs **once**, **automatically**, **when the class is first loaded into memory**, even **before any constructor or main method is called**.


```java
class AppConfig {
    static String version;

    static {
        version = "1.0.0";
        System.out.println("AppConfig 초기화 완료");
    }
}
```

# Why?
- When initializing static variables that need logic (not just direct assignment)
- loading native libraries
	- Some Java programs use `.dll` or `.so` files via `System.loadLibrary()`. Static blocks are perfect for loading them **once**, during class loading
	- `static { System.loadLibrary("nativeLib")}`
	- debugging

