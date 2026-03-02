# Dependency
>[!Dependeny]
>Refers to the **relationship that arises when one object uses another**. >

```java
public class OrderService {
    
    // 의존하고 있는 PaymentProcessor 객체를 직접 생성
    private PaymentProcessor paymentProcessor = new PaymentProcessor();
}
```
- Ex) If an `OrderService` uses a `PaymentProcessor`, we say that `OrderService` depends on `PaymentProcessor`.
- **Tight coupling**
	- Object is directly created leads to tight coupling
	- It's difficult to change implementations or use substitute objects (Mocks) for testing
# Dependency Injection
>[!Dependency Injection (의존성 주입)]
>Instead of an object creating its own dependencies, **its dependencies are given to it from the outside.**
>- A specific technique (a design pattern) that **implements** the **[[IoC (Inversion of Control)|Inversion of Control (IoC)]]** principle.

#### Without DI
```java
public class MenuController {
	public static void(String[] args) {
		MenuService menuService = new MenuService();
		List<Menu> menuList = menuService.getMenuList();
	}
}

//
public class MenuService {
	public List<Menu> getMenuList() {
		return null;
	}
}
```
- There is a dependency relationship established because the `MenuController` class creates and references another class object
#### With DI
```java
public class CafeClient {
	public static void main(String[] args) {
		MenuService menuService = new MenuService();
		MenuController controller = new MenuController(menuService); //
		List<Menu> menuList = menuService.getMenuList();
	}
}
public class MenuController {
	private MenuService menuService;

	/// HERE<<<< 
	public MenuController(MenuService menuService) {
		this.menuService = menuService;
	}
	
	public List<Menu> getMenus() {
		return menuService.getMenuList();
	}
	
}
```
- `MenuController` receives the `MenuService` object through its [[Constructor & this#Constructor|constructor]]
	- This is DI - passing an object as a constructor parameter = injecting an object from an external source (`CafeClient`)
	- `CafeClient` (the external source) passes `menuService` as a constructor parameter
# Loose coupling
>DI AVOIDS TIGHT COUPLING
- Tight coupling 
	- When you use the `new` keyword inside a class to create an object it depends on, you create a **strong, direct link** (tight coupling) to a specific implementation of that object.
	- U need to change *all* parts of the code that uses it if u edit the class
- Loose coupling
	- DI allows **loose coupling**, which allows you to easily substitute different implementations (e.g., *a real one for production, a stub for testing*) without *ever* changing the dependent class's internal code.

![[DI-interface.png]]
- `MenuController` depends only on the `MenuService` interface.
- The concrete implementations (`MenuServiceImpl`, `MenuServiceStub`) are injected at runtime.

# In Spring
- The Spring Framework's container streamlines dependency injection by *handling object creation and injection on your behalf*. 
	- Uses annotations like `@Component`, `@Service`, `@Repository` for **automatic component scanning** where Spring *directly instantiates and manages objects*.
		- [[⭐Layered Architecture]]
	- Uses `@Configuration` and `@Bean` methods when you want to **explicitly define how an object is created and configured** within Spring's container.
```java
@Configuration
public class Config {

    @Bean
    public MenuService menuService() {
        return new MenuServiceStub();
    }

    @Bean
    public MenuController menuController() {
        return new MenuController(menuService());
    }
}
```
- Spring uses the container to manage the **`MenuServiceStub` instance** (created by the `menuService()` method) as a [[Bean]].
- When creating the `menuController` [[Bean]], Spring supplies the managed `MenuServiceStub` instance as the `MenuService` dependency to `MenuController`'s constructor.**
# Ways to do it

| Method                      | Description                 | Recommendation                                   |
| :-------------------------- | :-------------------------- | :----------------------------------------------- |
| ⭐ **Constructor Injection** | Injects via the constructor | ✅ **Most Recommended**                           |
| Field Injection             | Injects via fields          | ❌ **Not Recommended (for testing/immutability)** |
| Setter Injection            | Injects via setter methods  | ⚠️ **Use for optional dependency injection**     |

#### Constructor Injection
- Constructor Injection is the most recommended
```java
public class OrderService {
    
    private final PaymentProcessor paymentProcessor;

    // 생성자 주입
    @Autowired
    public OrderService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }
}
```
- Advantage: You can use the `final` keyword
	- Means that you can guarantee that it will NEVER be `null`!! (you can avoid `NullPointerException`)
- When you have 2 or more constructors, you must use the `@Autowired` annotation on top of the constructor

#### Field Injection
```java
public class OrderService {

    // Field Injection - Not recommended for critical dependencies
    @Autowired
    private PaymentProcessor paymentProcessor;

    // No constructor needed to inject PaymentProcessor

    // ... other methods using paymentProcessor
}
```
- Not recommended
- You CANNOT use `final` keyword, there is a chance of it being `null` and causing `NullPointerException`
- It relies on **reflection** (a Spring mechanism) to set the field, which can make testing harder as you can't easily instantiate the object without Spring's container.

#### Setter Injection
```java
public class OrderService {

    private PaymentProcessor paymentProcessor;

    // Setter Injection
    @Autowired
    public void setPaymentProcessor(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    // ... other methods using paymentProcessor
}
```
- **When to Use:** It's useful for **optional dependencies** or for dependencies that might change during the object's lifecycle (though this is less common in typical Spring applications).
- Also not recommended for similar reasons like the field injection above
# Dependency in Gradle
>The project has dependencies to other libraries/modules
- Your software project isn't built from scratch in isolation. Instead, it relies on, or **depends on**, pre-written code or functionalities provided by other separate pieces of software
- [[Gradle#DI (Dependency Injection) Dependency Dependency Configuration|Gradle - Dependency configuration]]