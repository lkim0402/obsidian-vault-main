
>[!Overview]
>Validation is more than just checking request values in controllers; it involves comprehensively verifying domain object validity and business rule consistency. 
>- In Java [[Bean validation (Input validation)|bean validation]], annotations like `@Valid` and `@Validated` allow validation to be performed in various places. This section covers strategies using controllers, service layers, and validation groups.

- Basically
	- **Controllers** validate the _format_ and _required status_ of request parameters.
	- **Service layer** validations focus on _business rules_ and _relationships between objects_.
	- `@Validated` is useful when making use of **validation groups**.
    - For single-parameter validation, use `@Validated` only,  because `@Valid` only triggers validation if the target is a Java Bean.
- Along with your exception handling strategy, design how to send **error messages** in the response when validation fails.
# `@Valid` VS `@Validated`

| Aspect                                 | `@Valid`                                                       | `@Validated`                                                                                |
| -------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Defined in                             | `javax.validation.Valid` (JSR-380 standard)                    | `org.springframework.validation.annotation.Validated` (Spring Framework specific)           |
| Validation target                      | Object and nested objects                                      | Object and nested objects                                                                   |
| Validation group support               | ❌ Not supported                                                | ✅ Supported via `groups` attribute                                                          |
| Primary use                            | Basic field and nested object validation                       | Group-based validation and service-layer method validation                                  |
| Applicable locations                   | Controller parameters, fields, constructors                    | Controller/service methods, class level                                                     |
| Recursive validation of nested objects | ✅ Supported                                                    | ✅ Supported                                                                                 |
| Service method parameter validation    | ❌ Does not work                                                | ✅ Works via `@Validated` with Spring [[AOP (Aspect Oriented Programming)\|AOP]] interceptor |
| Declaration style                      | Simple `@Valid` annotation                                     | Can specify groups: `@Validated({Group.class})`                                             |
| Typical use case                       | Validate single request objects in controllers                 | Different validation constraints by scenario (e.g., create/update/delete)                   |
| Practical tip                          | Use `@Valid` on nested objects fields for them to be validated | Can apply validation directly on service method parameters                                  |
| Validation timing                      | Runtime, via JSR 380 ValidationProvider                        | Runtime, via Spring AOP-based interceptor                                                   |
- `@Valid`
	- Purpose: Checking *objects* - [[Validating Composite Objects]] 
		- Its purpose is to trigger a cascade of validations on the fields _within_ a passed object (`UserForm`), not on the parameter itself
	- Use for simple controller request body validation: [[⭐Handler Method Parameters#`@RequestBody`|Handler Method Parameters - @RequestBody]]
	- CANNOT use `@Valid` on simple parameters like `String` or `int` (you can use `@Validated`)
- `@Validated`
	- Purpose: `@Validated` is Spring's own, more capable version of `@Valid`
		- adds support for **group-based validation** (e.g., different rules for create/update)
	- Useful when:
	    - You want to validate method parameters in **service classes**
## `@Validated`
>Used for mainly 2 scenarios

#### For Group Validation in Controllers
- when you need to apply different sets of validation rules to the same object
```java
// Your DTO with validation groups
public class UserDto {
    public interface OnCreate {}
    public interface OnUpdate {}

    @Null(groups = OnCreate.class) // ID must be null when creating
    @NotNull(groups = OnUpdate.class) // ID must exist when updating
    private Long id;

    @NotBlank(groups = OnCreate.class) // Password is required for creation
    private String password;
}

@RestController
public class UserController {
    // Here, we ONLY apply the validation rules from the "OnCreate" group.
    @PostMapping("/users")
    public void createUser(@Validated(UserDto.OnCreate.class) @RequestBody UserDto userDto) {
        // ... logic to create user
    }
}
```

#### To Enable Method Validation in Services/Components
- if you put `@Validated` on top of a class (like a `@Service`), it enables validation for all public methods in that class
- By placing `@Validated` at the class level on any Spring bean (like a `@Service`, `@Component`, or `@Repository`), you "turn on" method validation for that bean. Spring will then automatically check for any validation annotations on the parameters of its methods.
- If a rule is violated, Spring will throw a `ConstraintViolationException`
	- [[Exception handling#`ConstraintViolationException`|Exception handling - ConstraintViolationException]]
```java
@Service
@Validated // "Turns on" method validation for this entire class
public class ProductService {

    // Spring will now automatically check that 'productId' is 1 or greater.
    // If not, a ConstraintViolationException is thrown.
    public Product findProductById(@Min(1) Long productId) {
        // ... logic to find a product
        return productRepository.findById(productId)
                .orElseThrow(() -> new ProductNotFoundException());
    }
}
```
#### `@Validated` DOES NOT WORK with static methods
- does not work on `static` methods

```java
@Service
@Validated // This enables method validation for the bean
public class MyValidationService {

    // THIS WORKS
    // Spring's proxy will intercept this call and validate the parameter.
    public String processData(@NotBlank String name) {
        return "Processing " + name;
    }

    // THIS IS IGNORED
    // This is a static method, so the proxy is bypassed. Validation will NOT run.
    public static String processStaticData(@NotBlank String name) {
        return "Processing static " + name;
    }
}
```

```java
// In another service...
@Autowired
private MyValidationService myValidationService;

public void runValidation() {
    // This call will throw a ConstraintViolationException, as expected.
    myValidationService.processData("  "); 

    // This call will SUCCEED. The @NotBlank is completely ignored.
    // It returns "Processing static  " with no exception.
    MyValidationService.processStaticData("  "); 
}
```
- Spring performs its "magic," like validation and transaction management, on **beans**, which are _instances_ of your classes that are managed by the [[IoC (Inversion of Control)#IoC Container|Spring container]]. It does this by creating a **proxy**—a dynamic wrapper object—around your actual bean.
- Why `static` is a problem
	- **`static` methods belong to the class itself, not to an instance**. 
	- When you call a `static` method, you are calling it on the class directly (`MyService.myStaticMethod()`), not on the instance managed by Spring.
	- The call completely bypasses the proxy, so Spring has no opportunity to intercept it and apply the validation logic
- [[@Transactional and the Self-Invocation Problem]]
- [[Spring AOP and Transactions]]

# `BindingResult`
- `BindingResult` is an object that stores validation error information and must be declared *immediately* after the parameter annotated with `@Valid` or `@Validated`.
- Mainly for:
	- Handling **external user input** in a `@Controller`.
	- Allows users to retry
```java
@PostMapping("/users")
public String createUser(@Valid UserForm userForm, BindingResult bindingResult, Model model) {
    if (bindingResult.hasErrors()) {
        return "users/form";  // On validation failure, return to the form view
    }
    userService.create(userForm);
    return "redirect:/users";  // On validation success, redirect to the users list
}
```
- When a request comes in, Spring tries to bind the incoming data (e.g., from a form) to your `UserForm` object. The `@Valid` annotation triggers the validation process based on the rules you defined in your `UserForm` class (like `@NotEmpty`, `@Email`, etc.).
- Instead of throwing an exception the moment a validation rule fails, Spring **catches all the errors and puts them into the `BindingResult` object.** Your controller method is then called, giving you a chance to check the clipboard.
- The main benefit is **control**
	- It prevents a `MethodArgumentNotValidException` from being thrown, which would normally result in a `400 Bad Request` error page. 
	- By checking `bindingResult.hasErrors()`, you can implement user-friendly logic
# In Service
- In the service layer, you can validate **more complex business rules** or perform **relationship validations** between domain objects, *beyond what the controller handles*. 
- It is common to apply *supplementary validation in the service layer when validation is difficult or insufficient in the controller*.
- [[⭐Layered Architecture#`@Service`|Layered architecture - Service]]

```java
public interface UserService {
    UserDto createUser(UserForm userForm);
    void transferMoney(Long sourceAccountId, Long targetAccountId, BigDecimal amount);
}

@Service
@Validated  // 클래스에 적용하여 메소드 파라미터 검증 활성화
public class UserServiceImpl implements UserService {

    @Override
    public UserDto createUser(@Valid UserForm userForm) {
        // 비즈니스 로직 수행
        return new UserDto();
    }

    @Override
    public void transferMoney(
        @NotNull @Min(1) Long sourceAccountId,
        @NotNull @Min(1) Long targetAccountId,
        @Positive BigDecimal amount) {

        // 같은 계좌로 송금 불가 검증
        if (sourceAccountId.equals(targetAccountId)) {
            throw new IllegalArgumentException("출발 계좌와 목적 계좌는 같을 수 없습니다");
        }

        // 복잡한 비즈니스 검증 로직...
    }
}
```

- Advantages
	- Easier testing by separating from the controller
	- Reusable complex business validations
	- Enables domain-centric separation of responsibilities

# In Controller (usually w/ `BindingResult`)
- In [[⭐Spring MVC|Spring MVC]], you can apply `@Valid` or `@Validated` to controller method parameters to automatically validate incoming request data. 
- If validation fails, you can use the `BindingResult` to check error details and return an appropriate response.
- [[⭐Layered Architecture#`@Controller` / `@RestController`|Layered architecture - Controller]]
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody UserForm userForm,
                                              BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            // 검증 오류 처리
            List<FieldError> errors = bindingResult.getFieldErrors();
            // 오류 처리 로직...
            return ResponseEntity.badRequest().build();
        }

        // 검증 성공 시 사용자 생성 로직
        UserDto savedUser = userService.createUser(userForm);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
}
```

# Using Validation Groups in Controller
- The same object *may require different validation rules depending on the context*. 
- For example, a field that must be `@NotNull` when creating a user may not be mandatory during update. In such cases, **Validation Groups** are used to apply different rules based on the situation.'

Interfaces
```java
public interface OnCreate {}
public interface OnUpdate {}
```

```java
public class UserForm {

    @NotNull(groups = {OnCreate.class, OnUpdate.class})
    private String name;

    @NotNull(groups = OnCreate.class)
    @Size(min = 8, groups = OnCreate.class)
    private String password;

    @NotNull(groups = OnCreate.class)
    @Email(groups = {OnCreate.class, OnUpdate.class})
    private String email;

    @Null(groups = OnCreate.class)
    @NotNull(groups = OnUpdate.class)
    private Long id;
}
```

In Controller
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping
    public ResponseEntity<UserDto> createUser(
            @Validated(OnCreate.class) @RequestBody UserForm userForm,
            BindingResult bindingResult) {
        // 생성 관련 검증 실행
    }

    @PutMapping("/{id}")
    public ResponseEntity<UserDto> updateUser(
            @PathVariable Long id,
            @Validated(OnUpdate.class) @RequestBody UserForm userForm,
            BindingResult bindingResult) {
        // 수정 관련 검증 실행
    }
}
```
- `@Validated(OnCreate.class)`
- `@Validated(OnUpdate.class)`