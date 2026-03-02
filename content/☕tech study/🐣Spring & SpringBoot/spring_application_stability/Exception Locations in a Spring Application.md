- [[Exception handling]]
- [[⭐Layered Architecture]]
- [[System exception VS Business Exception]] => repository exception VS service exception

| Layer          | Primary Responsibility                              | Exception Strategy                                                                                        |
| -------------- | --------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Domain**     | Guarantee the integrity of business rules.          | Throws exceptions if an operation would *violate its internal state* (e.g., `AlreadyWithdrawnException`). |
| **Controller** | Validate user requests and assemble responses.      | Throws exceptions for *invalid input*, like a `BadRequestException`.                                      |
| **Service**    | Orchestrate business logic and manage transactions. | Checks the state of domain entities and throws *business exceptions* (e.g., `MemberNotFoundException`).   |
| **Repository** | Access data from a persistent store.                | Converts low-level exceptions (e.g., `SQLException`) into Spring's `DataAccessException` hierarchy.       |
# Domain Exceptions
- The Domain Layer (your Entities or Value Objects) is responsible for protecting the core business rules of your system.
- To do this, it must *validate its own state* and throw an exception immediately if an operation would violate its integrity or "invariants."
- Why:
	- The domain entity must be responsible for its own **state invariants** (rules that must always be true).
	- If the Service Layer handled all of these checks, your business rules would be scattered, making the system hard to maintain.
	- This approach makes it easy to write focused **unit tests** for your domain logic

#### Example code
```java
public class Member {
    private MemberStatus status;

    public void withdraw() {
        // The domain object protects itself from invalid state changes.
        if (this.status == MemberStatus.WITHDRAWN) {
            // This prevents a duplicate call from changing the state incorrectly.
            throw new AlreadyWithdrawnException("This member has already been withdrawn.");
        }
        this.status = MemberStatus.WITHDRAWN;
    }
}
```

#### Examples of Domain-Level Custom Exceptions

|Exception Class|Condition for Throwing|
|---|---|
|`AlreadyWithdrawnException`|When a second withdrawal attempt is made on an already withdrawn account.|
|`InsufficientBalanceException`|When a withdrawal is requested for an amount greater than the current balance.|
|`InvalidOrderStatusException`|When a cancellation request is made for an order that has already been shipped.|
# Controller Exceptions
- The **Controller Layer** is the entry point for all [[Backend & Main Components Explained#Client-side vs Server-side|client requests]] and is responsible for returning responses. 
	- exception handling in this layer focuses on providing clear and appropriate error responses directly to the client
	- [[Bean validation (Input validation)]]  (e.g., errors from `@Valid` annotations) 
	- type mismatch
	- missing parameters
- characteristics
	- returns a structured error response combined with the appropriate **HTTP status code** (e.g., `400 Bad Request`, `404 Not Found`
	- uses **`@RestControllerAdvice`** and **`@ExceptionHandler`** to globally catch exceptions from all layers and convert them into a consistent client response
	- primary place to handle exceptions related to **input validation**
- Process:
	1. A client sends an invalid request (e.g., missing a required field).
	2. The framework, acting on the **Controller**, detects this problem and **throws** a specific exception (like `MethodArgumentNotValidException`).
	3. The exception propagates up.
	4. The **`GlobalExceptionHandler`** then **catches** this specific exception and formats a clean `400 Bad Request` response.
#### Example code
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    // 생성자 주입

    @PostMapping
    public ResponseEntity<UserResponse> createUser(@Valid @RequestBody UserRequest request) {
        User user = userService.createUser(request.toDto());
        return ResponseEntity.status(HttpStatus.CREATED).body(UserResponse.from(user));
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationException(MethodArgumentNotValidException e) {
        List<ErrorResponse.FieldError> fieldErrors = e.getBindingResult().getFieldErrors().stream()
            .map(error -> new ErrorResponse.FieldError(
                error.getField(),
                error.getRejectedValue() != null ? error.getRejectedValue().toString() : "",
                error.getDefaultMessage()
            ))
            .collect(Collectors.toList());

        ErrorResponse response = ErrorResponse.builder()
            .status(HttpStatus.BAD_REQUEST.value())
            .code(ErrorCode.INVALID_INPUT_VALUE.getCode())
            .message("입력값 검증에 실패했습니다.")
            .errors(fieldErrors)
            .build();

        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(response);
    }
}
```
# Service Exceptions
- [[System exception VS Business Exception]] => repository exception VS service exception
- role
	- either wrap low-level exceptions from the repository layer or throw business exceptions based on the state of your domain.
- benefits
	- You have full control over the **error code and message** that will eventually be returned to the client
	- You can control **transactional behavior**, ensuring that a transaction is rolled back when a business rule is violated
```java
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    // Constructor...
	
	@Transactional
    public Member getMember(Long id) {
        // Find the member by ID. If not found, throw our custom business exception.
        return memberRepository.findById(id)
                .orElseThrow(() -> new BusinessException(ErrorCode.MEMBER_NOT_FOUND));
    }
}
```
# Repository Exceptions
- The **Repository Layer** is responsible for all data access. 
	- When you use **Spring Data JPA** ([[Spring Data Access Technologies]]), the framework automatically handles most common exceptions for you. 
	- It provides an internal mechanism that catches low-level, technology-specific exceptions (like a database's `SQLException`) and converts them into Spring's own abstracted `DataAccessException` hierarchy. 
		- [[Exception handling#Exception Abstraction - `DataAccessException`|Exception handling: Exception Abstraction - DataAccessException]]
- Characteristics
	- automatic transation from db errors into a specific subset of `DataAccessException`
	- **Built-in Methods are Pre-handled**: When you use the standard methods provided by Spring Data JPA interfaces (like `findById()`, `save()`, `findAll()`), this exception translation is done for you automatically.
	- **Manual Handling is Sometimes Needed**: You only need to handle exceptions directly when you step outside the standard interface methods
		- custom repo implementation
		- when you're using the `EntityManger` to build complex queries
#### Example code (manual handling)
- The goal is to catch specific JPA exceptions (like `NoResultException` or `PersistenceException`) and convert them into an appropriate exception from Spring's `DataAccessException` hierarchy
```java
public interface OrderRepository extends JpaRepository<Order, Long>, OrderRepositoryCustom {
    // 기본 메서드들은 이미 예외 처리가 되어 있음
}

public interface OrderRepositoryCustom {
    // 커스텀 메서드 정의
    Order findWithItemsForUpdate(Long orderId);
}

@Repository
public class OrderRepositoryCustomImpl implements OrderRepositoryCustom {

    private final EntityManager entityManager;

    public OrderRepositoryCustomImpl(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    @Override
    public Order findWithItemsForUpdate(Long orderId) {
        try {
            return entityManager.createQuery(
                    "SELECT o FROM Order o JOIN FETCH o.items WHERE o.id = :id", Order.class)
                .setParameter("id", orderId)
                .setLockMode(LockModeType.PESSIMISTIC_WRITE)
                .getSingleResult();
        } catch (NoResultException e) {
            return null;
        } catch (PessimisticLockException e) {
            log.error("주문 ID {}에 대한 락 획득 실패", orderId, e);
            throw new DataAccessResourceFailureException("주문 데이터 락 획득 실패", e);
        } catch (QueryTimeoutException e) {
            log.error("주문 조회 쿼리 타임아웃", e);
            throw new QueryTimeoutException("주문 조회 시간 초과", e);
        }
    }
}
```
- flow
	1. **Catch JPA Exception:** The code catches `jakarta.persistence.PessimisticLockException`.
	2. **Throw Spring Exception:** It then throws a `org.springframework.dao.DataAccessResourceFailureException`, which **is** a part of the `DataAccessException` hierarchy.
- The service layer doesn't see the original `PessimisticLockException` from JPA. It only sees the new, more generic Spring exception that you threw from the repository, such as `DataAccessResourceFailureException` which is under the `DataAccessException` hierarchy

# Exception Translation Strategy
>[!Overview]
>**Exception Translation** is the practice of converting a low-level system exception (or an exception from a specific technology) into a more meaningful, application-specific exception.
>-  If you expose low-level exceptions directly to your business logic, your code becomes tightly coupled to that specific technology, which hurts maintainability and scalability.

- While frameworks like Spring provide this translation automatically for many data access technologies, it's a pattern you must also apply explicitly in your own code.
## Benefits

|Reason|Explanation|
|---|---|
|**Technology Independence**|If you expose specific exceptions from JDBC, JPA, or file I/O, you have to change your service layer code every time the underlying technology changes.|
|**Clearer Meaning**|`IOException` only tells you "a file error occurred." A `BusinessException` can provide a specific business meaning, like "failed to load user profile."|
|**Consistent Client Responses**|It allows you to provide consistent HTTP status codes and meaningful error messages to the client, without exposing your internal technology stack.|
|**Easier Testing**|By creating a translation layer, you can decouple your business logic from the data access technology, allowing for purer and simpler unit tests.|
## Example: reading from file in repository
- catches the low-level `IOException` and translates it
```java
// A repository that loads user data from the file system
public class FileUserRepository {
    public User load(String userId) {
        try {
            // Logic to load user info from a file
            return loadFromFile(userId);
        } catch (IOException e) {
            // Translate the low-level system exception into a more abstract application exception
            throw new DataAccessException("Could not read the user file.", e);
        }
    }
}
```

- flow
	1. **`IOException`**: A low-level system exception from the Java I/O library.
	2. ➡️ **`DataAccessException`**: Translated in the **Repository Layer**. This is a more abstract exception that hides the fact that the data came from a file.
	3. ➡️ **`BusinessException`**: The **Service Layer** might catch the `DataAccessException` and wrap it again in a high-level business exception (like `UserLoadFailedException`) that has specific meaning for the application's domain.

## Key considerations
- **Include the Original Exception (`cause`)**: This is critical for debugging. When you throw your new exception, always include the original one as the `cause` (e.g., `throw new DataAccessException("message", e);`).
- **Respect Abstraction Layers**: The Repository should only handle technology-related exceptions (like `IOException`). The Service layer should be responsible for business-related decisions and exceptions.
- **Structure Exception Messages**: Ensure the new, translated exception contains enough context and a clear log message to be useful for debugging.

## Tips
- Log exceptions both at the moment of **translation** (e.g., in the repository) and at the final **handling point** (e.g., in the `GlobalExceptionHandler`) to ensure you have a complete trace.
	- [[Exception handling#⭐ `GlobalExceptionHandler`|⭐Exception handling - GlobalExceptionHandler]]
- Write clear error messages that distinguish between **technical failures** (e.g., "database connection failed") and **business rule violations** (e.g., "user not found").
- Adopt a pattern where translated exceptions are always thrown with a specific `ErrorCode` from your enum.

Summary of how exceptions are typically translated as they move up through the application layers:

|Layer|Catches (Low-Level)|Throws (Translated)|
|---|---|---|
|**Repository**|`IOException`, `SQLException`, `PersistenceException`|`DataAccessException`|
|**Service**|`DataAccessException`|`BusinessException`|
|**Controller**|`BusinessException`, `ValidationException`|`ErrorResponse` (as JSON)|