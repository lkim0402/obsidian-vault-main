
>[!Overview]
>The most widely used mocking library that provides easy ways to **create mock objects, define their behavior, and verify method calls**.
>- Basically creating _fake_ versions of dependencies
>- [[🐣Spring & SpringBoot|Spring Framework]] integrates naturally with Mockito through features like `@MockBean`.
    
- Test doubles = stub, mock, spy
# Syntax
- `when()` 
	- giving the Assistant **instructions _before_** the work starts => *when* this happens, *return* this
	- telling your mock object what to do when *one of its methods is called*
	- setting up a fake response
	- happens in `// given`
	- `when(mockProductRepository.getPrice("product-123")).thenReturn(50.0);`
	- chained with
		- `thenReturn`
		- `thenThrow`
		- etc
- `verify()` 
	- checking if a method on your mock object was actually called
	- This is how you confirm that your code is behaving correctly
	- Happens in `// then`
# Stub vs Mock vs Spy ⭐
| Item               | Stub                                                    | Mock                                                                               | Spy                                                                                         |
| ------------------ | ------------------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Purpose**        | State Verification<br>- Return _fixed data_             | Behavior Verification<br>- Verify _calls_                                          | Partial Mocking<br>- Real object + override parts                                           |
| What I want        | I want to pre-set the data or state needed for testing. | I want to verify that the methods of an external dependency were called correctly. | I want to use most of the real object’s logic but replace only specific methods with fakes. |
| **Focus**          | State (pre-set responses)                               | Behavior (calls, args, order)                                                      | Partial mocking                                                                             |
| **Implementation** | Hard-code / `when(...).thenReturn(...)`                 | - `mock()` → empty shell<br>- `mock(...)` + `verify(...)`                          | - `spy()` → wraps real object<br>- both `when(...).thenReturn(...)` and `verify(...)`       |
| **Use Case**       | Provide canned data (repo lookup)                       | Check external interactions (email, DB save)                                       | Test legacy/hard-to-isolate code                                                            |
| **Scope**          | Mostly [[Unit Test\|unit tests]]                        | [[Unit Test]] / [[Slice Test]]                                                     | [[Unit Test]] / [[Integration Test]]                                                        |
## Stub = Just Setting Return Values
- A **Stub** is the simplest form of a test double. Its only job is to return pre-arranged data when it's called.
- just think it's an actor who only says their pre-written lines, no matter what. 🎭

```java
@Test
void whenUserIdExists_thenUserNameShouldBeReturned() {
    // ========================= given =========================
    // 1. Create a Stub object and define its behavior
    UserRepository stubUserRepository = mock(UserRepository.class);
    User fakeUser = new User("luckykim", "김러키");
    // Configure it to always return fakeUser when findById is called with the ID "luckykim"
    when(stubUserRepository.findById("luckykim")).thenReturn(Optional.of(fakeUser));
    
    // 2. Inject the Stub into the object under test
    UserService userService = new UserService(stubUserRepository);

    // ========================= when =========================
    // The service method will receive the fakeUser from our stub
    String userName = userService.getUserName("luckykim");

    // ========================= then =========================
    // Verify that the correct name from the stubbed user ("김러키") was returned
    assertThat(userName).isEqualTo("김러키");
}
```
- **Situation:** You need to test a piece of your service logic that can only proceed after `userRepository.findById()` returns a specific `User` object. In this case, the **value being returned** is what's important for the test.
- If your test simply needs a dependency to return a pre-defined value, you should use a **stub**. (In Mockito, you create a stub by using `when().thenReturn()` on a mock object.)
## Mock = Fake Object
- Includes the role of a Stub, but it goes further by also **verifying the behavior** of an object
- It can check if
	- a specific method was called as expected
	- how many times it was called
	- with what arguments, etc
```java
@Test
void whenUserRegisters_thenWelcomeEmailShouldBeSent() {
    // ========================= given =========================
    // 1. Create mock objects
    EmailService mockEmailService = mock(EmailService.class);
    // This mock is only used to provide a necessary dependency, acting as a Stub
    UserRepository stubUserRepository = mock(UserRepository.class);

    // 2. Inject mocks into the object under test
    UserService userService = new UserService(stubUserRepository, mockEmailService);

    // ========================= when =========================
    userService.register("user@example.com", "password123");

    // ========================= then =========================
    // Verify that the sendEmail method of mockEmailService was called
    // exactly 1 time with the arguments "user@example.com" and "가입을 환영합니다" (Welcome aboard).
    verify(mockEmailService, times(1)).sendEmail("user@example.com", "Welcome aboard");
}
```
- **Situation:** After a user registration process completes, an external system method like `emailService.send()` or `notificationManager.notify()` **must be called exactly once** with the correct content. The return value is `void`, so it isn't important.
- If you find yourself asking, "So... was the method actually called? How many times? What were the arguments?" — then you should use a **mock** and check it with `verify`.
## Spy= Real Object + Ability to Track/Override
A **Spy** is created by **wrapping a real object**. By default, all method calls on the spy are delegated to the real object's logic. However, you can selectively replace the behavior of specific methods when needed.
```java
@Test
void whenOrderIsPlaced_thenPaymentShouldBeProcessedAndOrderIdGenerated() {
    // ========================= given =========================
    // 1. Create a Spy object based on a real instance
    OrderService realService = new OrderService();
    OrderService spyOrderService = spy(realService);
    
    // 2. Stub only a specific method
    // **Caution**: Because a spy calls the real method by default, you must use the 
    // doReturn(...).when(spy).method() syntax to be safe, instead of when(spy.method())...
    doReturn(true).when(spyOrderService).processPayment(any(Order.class));
    
    Order newOrder = new Order("item-123", 10000);

    // ========================= when =========================
    // This will execute the real logic of placeOrder(), but the call to
    // processPayment() inside it will be intercepted and return true.
    boolean isOrderSuccessful = spyOrderService.placeOrder(newOrder);

    // ========================= then =========================
    // 1. Verify the final result
    assertThat(isOrderSuccessful).isTrue();
    
    // 2. Verify the state to confirm the real method was called
    assertThat(newOrder.getOrderId()).isNotNull(); 
    
    // 3. Verify the behavior to confirm the stubbed method was called
    verify(spyOrderService, times(1)).processPayment(newOrder);
}
```
- **Situation:** You want to test a class (`OrderService`) that performs complex logic by calling 10 of its own methods internally. You want to test all of this real logic, but just **one** of those methods, `processPayment()`, is too difficult to test because it calls an external payment API.
- If you want to use **99% of the real object as-is** and only **change 1% of its behavior**, you should use a **spy**.

## What to use?
Follow this decision process: What is my primary goal?
- *A) To check the final return value or state of my object?*
	- Your method returns a value (e.g., `String`, `boolean`, `User`). You need dependencies to provide canned data.
	- ➡️ Use a Stub.
- *B) To check that a dependency was called correctly?*
	- Your method calls another service and returns `void` (e.g., sending an email, logging a message). The _interaction_ is the result.
	- ➡️ Use a Mock.
- *C) To test a real object's logic, but one small part of it is problematic (e.g., a real network call)?*
    - You want most methods to execute real code, but you need to fake just one or two methods.
    - ➡️ Use a Spy. This is a special case. If you don't fit this description, stick with Mocks or Stubs.
# Matchers
- **instructions** you give to your test doubles
	- **Specific Instance (`specificClass`):** The script says, "You must interact with _this specific class."
    - **General Class (`any(someClass.class)`):** The script says, "You must interact with *any class*—I don't care who."
#### `any(someClass.class)`
1. Use when the object is created _inside_ the service method, so you don't have a reference to it in your test
	- the service created the new `binaryContent` and is using it in the method
```java
// In Service: new BinaryContent(...) is created internally
// In Test: Check the behavior
// Script Check: "Did you save *some kind of* BinaryContent object? I don't know the specifics."
verify(binaryContentRepository).save(any(BinaryContent.class));
```

2. You don't care about the exact object, just that the method was called
```java
verify(userRepository).save(any(User.class)); // ✅ "Just verify save was called with some User"
```

#### `specificInstance`
- Use when you have the exact object reference and the specific values are important to verify
```java
// Script Check: "Did you send an email with *exactly these* arguments?"
verify(mockEmailService).sendEmail("user@example.com", "Welcome aboard");
```

# Creating Mocks

| API            | Description                                                                  |
| -------------- | ---------------------------------------------------------------------------- |
| `mock()`       | Static method to manually create a mock object                               |
| `@Mock`        | Declaratively creates a mock object automatically                            |
| `@MockBean`    | Registers a mock object in the Spring context (for integration tests)        |
- Use `mock()` for simple mock creation, `@Mock` for declarative mock creation, and `@InjectMocks` for automatic dependency injection.
## `Mockito.mock()`
- most basic way
```java
import static org.mockito.Mockito.mock;

@Test
void testWithMockMethod() {
    UserRepository userRepository = mock(UserRepository.class);
    // userRepository를 사용한 테스트 코드
}
```
- **Advantages**: The code flow is explicit, and it can be used without any extra configuration.
- **Disadvantages**: You must inject dependencies manually, which can make the test class more complex.
## `@ExtendWith + @Mock`
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @Test
    void testWithMockAnnotation() {
        // userRepository has already been initialized as a mock object
    }
}
```
- most common (standard way)
- *Creates a mock object* in a declarative way 
- Need to DI ourselves ([[DI (Dependency Injection)]])
- Used together with `@ExtendWith(MockitoExtension.class)`.
##  `@MockBean` 
An annotation used in **Spring Boot tests** that replaces a bean registered in the Spring context with a mock.
```java
@SpringBootTest
class UserServiceIntegrationTest {

    @MockBean
    private UserRepository userRepository;

    @Autowired
    private UserService userService; // mockRepository is automatically injected
}
```
- **Characteristic**: Useful in [[Integration Test|integration tests]] that run with the real application context, since it affects the actual Spring context.
	- In Spring Boot integration tests, `@MockBean` registers and injects mocks into the application context.
	- [[IoC (Inversion of Control)#`ApplicationContext`|IoC container - ApplicationContext]]

# Mock Object Injection Techniques
- [[DI (Dependency Injection)]]
- This is the next step

| API            | Description                                                                  |
| -------------- | ---------------------------------------------------------------------------- |
| `@InjectMocks` | Automatically injects mock dependencies into the class under test            |

## Constructor injection
```java
@Test
void testWithConstructorInjection() {
    UserRepository mockRepository = mock(UserRepository.class);
    UserService userService = new UserService(mockRepository);

    // Test using userService
}
```
- The mock is manually passed through the constructor when instantiating the class under test.
## `@Mock + @InjectMocks` (preferred for [[Unit Test|unit tests]])
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    void testWithInjectMocks() {
        // userService already has mockRepository injected
    }
}
```
- `@InjectMocks` 
	- Automatically injects objects created with `@Mock` into the class under test.
	- automatically determines injection order, making test code relatively flexible against dependency changes
- Eliminates the need for manual constructor or setter calls for mock injection.
## Handling Complex Dependency Structures
```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock
    private ProductRepository productRepository;
    @Mock
    private UserRepository userRepository;
    @Mock
    private PaymentGateway paymentGateway;

    @InjectMocks
    private ProductService productService;
    @InjectMocks
    private UserService userService;
    @InjectMocks
    private OrderService orderService;

    @Test
    void testComplexDependencies() {
        // Automatic injection works even in complex service-repository structures
    }
}
```
- You can create mock objects across multiple layers and automate injection between these layers.
## `@MockBean + @Autowired` (preferred for [[Integration Test|integration tests]]) 
```java
@SpringBootTest
class UserServiceIntegrationTest {

    @MockBean
    private UserRepository userRepository;

    @Autowired
    private UserService userService; // mockRepository is injected from the Spring context
}
```
- `@MockBean` is used only in [[Integration Test|integration tests]]. 
- For [[Unit Test|unit tests]], the combination of `@Mock` and `@InjectMocks` is recommended.

# Syntax
