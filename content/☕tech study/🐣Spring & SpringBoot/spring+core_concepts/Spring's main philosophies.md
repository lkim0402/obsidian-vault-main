# POJO Based Development
>[!summary]
>Spring's core philosophy is "**Non-Invasive (기능은 많되, 개입은 적게)**." POJO-based development is key to this, freeing developers from technical ties and allowing focus on business logic. This is fundamental to Spring.

![[pojo.png|300]]
- [[POJO vs JavaBeans VS Spring Beans#POJO (Plain Old Java Object)|POJO (Plain Old Java Object)]]
- Spring leverages POJO philosophy for flexible architectures while maintaining Java's simplicity (Martin Fowler)
- The reason Spring chose POJO
	- **Eliminates container dependency** -> Objects executable outside the Spring container
	- **Separate function & implementation** -> Maintain pure business logic independent of tech stack. 
	- **Ease of Unit Testing** -> Remove external dependencies for independent testing
#### Advantages
- **Achieves Technology Independence**
	- Business logic is not tied to specific technologies, making tech stack changes or portability easier.
- **Maintains Code Purity**
	- Example: Transaction handling can be added with just `@Transactional` annotation; *core logic remains POJO*.
- **Ease of Testing**
	- POJOs can be created and run independently with just a [[Java Virtual Machine (JVM)|JVM]], no separate container needed.
	- Excellent for unit testing, mocking, and test automation.

# Object-Oriented Design Principles
>[!Why]
>The Spring Framework isn't just a tech stack; it's a development philosophy that actualizes core software design principles, especially Object-Oriented Design Principles.

## DRY (Don't Repeat Yourself)
- **Extracting Common Functionality → AOP (Aspect-Oriented Programming)**
	- Defines repetitive logic (e.g., transactions, logging, authentication) as common concerns.
- **Configuration Reuse**
	- Abstraction through profiles, bean configuration classes, etc., simplifying XML/annotation settings.
	- Example: Using `@Transactional`, `@ComponentScan` to remove repetitive tasks via simple declarations
- **Example - Extracting the logging functions**
	- Used with Spring AOP (Common Logic Separation)
```java
public class OrderService {
    public void createOrder() {
        System.out.println("[LOG] createOrder called");
        // 핵심 로직
    }
    public void cancelOrder() {
        System.out.println("[LOG] cancelOrder called");
        // 핵심 로직
    }
}

======================================================
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.OrderService.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("[LOG] " + joinPoint.getSignature().getName() + " called");
    }
}
@Service
public class OrderService {
    public void createOrder() { /* 핵심 로직 */ }
    public void cancelOrder() { /* 핵심 로직 */ }
}
```
## SRP (Single Responsibility Principle)
>[!SRP with spring]
>[[SRP(Single Responsibility Principle)]] states: "A class should have only one reason to change." 
>- The **'Reason to change'** defines its responsibility. 
>- Spring's [[⭐Layered Architecture]] (`Controller` ↔️ `Service` ↔️ `Repository`) facilitates separating business logic and assigning single responsibilities to each layer.

#### SRP Implementation with Spring
- **Controller:** Handles only HTTP request/response.
- **Service:** Focuses on business logic.
- **Repository:** Dedicated to data access (DAO).
- Dependency Injection (DI) further clarifies role separation between layers.
#### Pre-SRP (All logic in Controller)
```java
@RestController
public class MemberController {
    @Autowired private MemberRepository repository;

    @PostMapping("/members")
    public ResponseEntity<?> register(@RequestBody MemberDto dto) {
        Member member = new Member(dto.getName());
        repository.save(member);
        return ResponseEntity.ok("saved");
    }
}
```
#### **With SRP Applied:**
```Java
// Controller handles request, delegates business logic to Service
@RestController
public class MemberController {
    @Autowired private MemberService service;

    @PostMapping("/members")
    public ResponseEntity<?> register(@RequestBody MemberDto dto) {
        service.register(dto);
        return ResponseEntity.ok("saved");
    }
}

// Service handles business logic, uses Repository for data
@Service
public class MemberService {
    @Autowired private MemberRepository repository;

    public void register(MemberDto dto) {
        Member member = new Member(dto.getName());
        repository.save(member);
    }
}
```

- As a side note, a large number of constructor arguments is a bad code smell, implying that the class likely has too many responsibilities and should be refactored to better address proper separation of concerns.
## SoC (Separation of Concerns)
>[!Separation of concerns]
>SoC is a design principle where system modules are each responsible for **different concerns**. Each module focuses on its core function, delegating other aspects to different components.
- Very Related: [[AOP (Aspect Oriented Programming)]]
- Spring supports SoC through:
	- **[[AOP (Aspect Oriented Programming)]]:** Separates cross-cutting concerns (e.g., transactions, logging, security).
	- [[DI (Dependency Injection)]]: Separates concerns about implementation classes *externally*.
	- **[[IoC (Inversion of Control)#`ApplicationContext`|ApplicationContext]]:** Separates configuration information and object creation 
#### Example
SoC leads to highly modular, testable, and maintainable Spring applications.
- Example: Using `@Aspect` and `@Around` to separate logging outside business logic.
- Example: Clearly defining component roles with `@Service` and `@Repository`.
#### Pre-SoC (Business logic mixed with transaction/security)
```java
public class PaymentService {
    public void pay(User user) {
        if (!user.hasPermission("PAY")) throw new SecurityException(); // Security mixed
        try {
            beginTransaction(); // Transaction mixed
            // Payment processing core logic
            commitTransaction();
        } catch (Exception e) {
            rollbackTransaction(); // Transaction mixed
        }
    }
}
```

#### With SoC Applied (Concerns Separated)
```java
// Security logic separated into an Aspect
@Aspect
@Component
public class SecurityAspect {
    @Before("execution(* com.example.PaymentService.pay(..))")
    public void checkPermission(JoinPoint jp) {
        // Security check logic here
    }
}

// Transaction handled by @Transactional annotation
@Service
@Transactional
public class PaymentService {
    public void pay(User user) {
        // Only core business logic remains
    }
}
```


# Test-Driven Development (TDD)
>[!note]
>[[🐣Spring & SpringBoot|Spring]] is a highly suitable framework for implementing TDD due to its [[Spring's main philosophies#POJO Based Development|POJO-based design]], Dependency Injection (DI), container independence, and provision of testing utilities.
- [[Test-Driven Development (TDD)]]
#### Ease of Unit Testing
- Spring's architecture (based on IoC/DI) lowers coupling between objects, supporting an environment where each object can be independently created and tested
	- **POJO-based Design:** Spring components can run as simple Java objects (POJOs) *without a container*, allowing easy unit testing with standard frameworks like *JUnit*.
	- **Dependency Injection (DI):** Test-target objects receive dependencies *externally* instead of creating them internally, *allowing easy integration with mocking tools like Mockito*.
```java
@Mock private MemberRepository memberRepository;
@InjectMocks private MemberService memberService;
```
#### Integration Test Support
Spring offers robust support for configuring integration test environments, not just unit tests.
- **`@SpringBootTest` Annotation:** Enables testing the entire Spring Boot application at the context level, mimicking the actual application runtime environment
- **TestContext Framework:** Spring includes its own `TestContext` Framework, which automates context initialization, transaction handling, and configuration class loading before and after tests.
```java
@SpringBootTest
class MemberServiceIntegrationTest {
    @Autowired private MemberService memberService;
    // ...
}
```