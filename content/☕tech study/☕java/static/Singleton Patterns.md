- A popular design pattern where you only need ONE single instance in the entire application
- use `static` to share the instance
```java
class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton() {} // 외부 생성 차단

    public static Singleton getInstance() {
        return instance;
    }
}

// 사용
Singleton s = Singleton.getInstance();
```

- make the [[Constructor & this#Constructor|constructor]] private manage the instance with the static field
	- this guarantees that this is the only instance.. you can't use the `new` keyword with this!
	- In Spring, the objects will often all be singletons