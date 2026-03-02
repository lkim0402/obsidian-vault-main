
>[!Exception Handling]
>- **Exception**
>	- an unexpected or abnormal event that can occur while a program is running
>- **Exception handling**
>	- a programming technique for responding to these events to maintain the system's normal operation and provide appropriate feedback to the user
>- Without it, a single, simple error could cascade and lead to a complete service outage

- [[Exception handling Structure & Hierarchy]]
	- [[⭐Layered Architecture|in a layered architecture]]
	- exception hierarchy
- [[System exception VS Business Exception]]
- [[Exception Locations in a Spring Application]]
# Why?
- Usually tomcat provides default exception handling even if we don't implement our own -> lacks detail & very generic
	- so we much create *custom exceptions* that are specific to our application's business logic
- Benefits
	- **Consistent responses** - Provide a unified error format so the [[Backend & Main Components Explained#Client-side vs Server-side|client]] can handle failures in a predictable way.
	- **Clear msgs** - Deliver specific, distinct messages for different error types (e.g., business logic failure, bad input, system error).
	- **Faster debugging** - easier to find the source of the problem
	- enhanced security
- If not handled properly
	- **Service Downtime** - An unhandled exception can cause the entire server or application thread to crash
	- **Poor User Experience** - Users are shown unhelpful and confusing responses, like a generic HTTP 500 error page
	- **Difficult Bug Tracking** - Without proper exception logs, it becomes incredibly difficult to figure out the root cause of a failure
	- **Security Risks** -Exposing detailed stack traces in an error message can reveal sensitive information about your application's architecture.
- It's good to add [[Logging]] when throwing exceptions
# Key Exception Handling Strategies

| Strategy                           | Description                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Global Exception Handling** <br> | Applies **global exception handling** across the entire application, so you don't have to repeat code in every controller.<br>- `@ControllerAdvice`<br>- `@RestControllerAdvice`                                                                                                                                                     |
| **Custom Exceptions**              | Define your own specific exceptions for business-related errors (e.g., `InsufficientFundsException`).<br>- makes your code clearer and your error handling more precise<br>- Create custom exceptions for *each part of your business domain* (e.g., `UserException`, `OrderException`) to make your code more specific and readable |
| **Appropriate HTTP Status Codes**  | Return the correct HTTP status code to the client<br>- Build a clear management system for error codes and messages, typically by using an Enum.<br>- [[Exception handling Structure & Hierarchy#`Errorcode.java`\|Exception handling Structure & Hierarchy - Errorcode.java]]<br>- [[Systematizing Error Codes]]                    |
| **Logging Integration**            | Connect your exception handling to a logger. When an error occurs, this records detailed information (like the stack trace) to a log file, which is essential for quick debugging. 🔍<br>- [[Logging]]                                                                                                                               |
- Use a standard `ErrorResponse` class to ensure all error responses follow the same JSON format.
- You can also use a **Static Factory Method Pattern** 
	- commonly used to create error handler methods.
    - Allows you to centralize and standardize error responses for different exception types
    - Helps handle specific error scenarios cleanly without duplicating code
# `GlobalExceptionHandler`
- Global Exception Handling => Handles exceptions from **ALL** controllers across your entire application
- [[Exception handling Structure & Hierarchy#If you use `GlobalExceptionHandler`|Exception handling Structure & Hierarchy - GlobalExceptionHandler]]
#### The Journey of an Unchecked Exception
- The Big Picture
	- You use an **unchecked** exception so it can fly past the controller without being caught.
	- `@Transactional` uses the **unchecked** exception as a signal to **ROLLBACK** the database transaction.
		- [[Transaction]]
	- The [[⭐Layered Architecture#`@Controller` / `@RestController`|controller]] ignores the exception because handling system errors isn't its job.
	- The **`GlobalExceptionHandler`** acts as a final safety net, catches the exception, and provides a clean error response to the [[Backend & Main Components Explained#Client-side vs Server-side|client]]
#### `@ControllerAdvice`  (traditional)
- you have to manually add `@ResponseBody` to each handler method if you want to return a JSON response
- It's more flexible, as it can also be used to return a `ModelAndView` to render an HTML error page, but this is less common for APIs
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(CustomException.class)
    @ResponseBody // You need to add this manually
    public ErrorResponse handleCustomException(CustomException ex) {
        return new ErrorResponse(...);
    }
}
```

#### `@RestControllerAdvice` (REST API)
- `@RestControllerAdvice` is simply a convenience annotation that combines `@ControllerAdvice` and `@ResponseBody`.
- automatically serializes the return value of its methods into the HTTP response body usually as JSON
	- standard choice for building [[REST API|REST APIs]]
- [[Exception handling Structure & Hierarchy#Basic Design (components)|Exception handling Structure & Hierarchy - Basic Design (components)]]
```java
@RestControllerAdvice  
public class GlobalExceptionHandler {  
  
  @ExceptionHandler(NoSuchElementException.class)  
  public ResponseEntity<ErrorResponse> handleException(NoSuchElementException e) {  
    ErrorResponse errorResponse = new ErrorResponse(HttpStatus.NOT_FOUND.value(), e.getMessage());  
    return ResponseEntity  
        .status(HttpStatus.NOT_FOUND)  
        .body(errorResponse);  
  }  
  
  @ExceptionHandler(IllegalArgumentException.class)  
  public ResponseEntity<ErrorResponse> handleException(IllegalArgumentException e) {  
    ErrorResponse errorResponse = new ErrorResponse(HttpStatus.BAD_REQUEST.value(), e.getMessage());  
    return ResponseEntity  
        .status(HttpStatus.BAD_REQUEST)  
        .body(errorResponse);  
  }  
  
  @ExceptionHandler(Exception.class)  
  public ResponseEntity<String> handleException(Exception e) {  
    return ResponseEntity  
        .status(HttpStatus.INTERNAL_SERVER_ERROR)  
        .body(e.getMessage());  
  }  
  
  @ExceptionHandler(FileAccessException.class)  
  public ResponseEntity<String> handleFileAccessException(FileAccessException e) {  
    return ResponseEntity  
        .status(e.getStatus())  
        .body(e.getMessage());  
  }  
}
```
- You can put these `@ExceptionHandler`s in the controller layer, but it's not recommended

# `ConstraintViolationException`
- [[Input validation locations (service, controller)#`@Validated`|Input validation locations (service, controller) -@Validated]]
- what it is
	- When you put validation on a service method (e.g., `findUserById(@Min(1) Long id)`), you are setting up an internal security checkpoint. 
	- You're saying, "No part of our own application should ever be allowed to pass through this gate with an ID less than 1."
	- It's not correctable user input, it's a *bug made by the devs* => so it throws this exception, halting execution
- When you catch a `ConstraintViolationException`, it contains a set of `ConstraintViolation` objects
	- Each individual violation object is like a detailed "violation report"
	- you can examine this with
		- `getPropertyPath` ->tells you **which field or parameter failed validation**
		- `getInvalidValue()`-> returns the **actual bad data** that was submitted and caused the validation to fail
		- `getMessage()`->gives you the **human-readable error message** that describes the validation rule that was broken
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ConstraintViolationException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public Map<String, Object> handleConstraintViolationException(ConstraintViolationException ex) {
        
        Map<String, Object> errorResponse = new HashMap<>();
        errorResponse.put("message", "Validation failed");

        // Loop through each individual validation error
        List<Map<String, String>> errors = ex.getConstraintViolations().stream()
                .map(violation -> {
                    Map<String, String> errorDetails = new HashMap<>();
                    // Use the methods to get the details
                    errorDetails.put("field", violation.getPropertyPath().toString());
                    errorDetails.put("rejectedValue", violation.getInvalidValue().toString());
                    errorDetails.put("reason", violation.getMessage());
                    return errorDetails;
                })
                .collect(Collectors.toList());

        errorResponse.put("errors", errors);
        return errorResponse;
    }
}
```

If you called `userService.findUserById(-5L)`, this handler would catch the exception and produce a clean JSON response like this:
```json
{
  "message": "Validation failed",
  "errors": [
    {
      "field": "findUserById.userId",
      "rejectedValue": "-5",
      "reason": "must be greater than or equal to 1"
    }
  ]
}
```
# Custom Exceptions 
- without this
	- lack of clarity 
	- difficult debugging
	- inflexible handling
	- inconsistent responses (no consistent, structured error response)
- used for [[System exception VS Business Exception#Business Exception|business exceptions (service layer)]]

#### Example ⭐
```java
public class BusinessException extends RuntimeException {
    private final ErrorCode errorCode;

    public BusinessException(ErrorCode errorCode) {
        super(errorCode.getMessage());
        this.errorCode = errorCode;
    }

    public ErrorCode getErrorCode() {
        return errorCode;
    }
}
```

```java
import org.springframework.http.HttpStatus;

public enum ErrorCode {
    OUT_OF_STOCK(HttpStatus.CONFLICT, "E001", "Not enough stock available."),
    DUPLICATE_ORDER(HttpStatus.CONFLICT, "E002", "This is a duplicate order.");

    private final HttpStatus status;
    private final String code;
    private final String message;

    ErrorCode(HttpStatus status, String code, String message) {
        this.status = status;
        this.code = code;
        this.message = message;
    }

    // Getters...
    public HttpStatus getStatus() { return status; }
    public String getCode() { return code; }
    public String getMessage() { return message; }
}
```

In service
```java
public void placeOrder(String productId, int quantity) {
	Product product = productRepository.findById(productId);

	// Check the business rule
	if (product.getStock() < quantity) {
		// If the rule fails, throw the custom exception
		throw new BusinessException(ErrorCode.OUT_OF_STOCK);
	}
	// ... continue with normal order processing
}
```

ErrorResponse
```java
public class ErrorResponse {
    private final String code;
    private final String message;
    private final long timestamp;

    public ErrorResponse(String code, String message) {
        this.code = code;
        this.message = message;
        this.timestamp = System.currentTimeMillis();
    }
    // Getters...
}
```
With `GlobalExceptionHandler`
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    // This method will catch any BusinessException thrown in the application
    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ErrorResponse> handleBusinessException(BusinessException ex) {
        // 1. Get the specific error code from the exception
        ErrorCode errorCode = ex.getErrorCode();

        // 2. Create the error response DTO
        ErrorResponse response = new ErrorResponse(errorCode.getCode(), errorCode.getMessage());

        // 3. Return an HTTP response with the correct status and body
        return new ResponseEntity<>(response, errorCode.getStatus());
    }
}
```

# Static Factory Method Pattern
- Instead of making an object like this (using a constructor): `ErrorResponse response = new ErrorResponse(status, message, errors);`
	- You create it by calling a `static` method on the class itself: `ErrorResponse response = ErrorResponse.of(bindingResult);`
-  `of` vs. `from` Convention
	- `of`
		- Often used to **aggregate** or create an object from its component parts. You are creating an object _of_ these parameters
		- used when there's one or more parameters
	- `from` 
		- Often used to signal a **type conversion**. You are creating an object _from_ another, different type of object. For example, `Instant.from(zonedDateTime)`.
		- usually used for one parameter
Example usage
```java
// Assume you have a caught 'bindingResult' from a controller validation failure
ErrorResponse responseForFormError = ErrorResponse.of(bindingResult);

// Assume you have caught a 'violations' set from a ConstraintViolationException
ErrorResponse responseForMethodError = ErrorResponse.of(violations);

// Creating a simple, custom error response
ErrorResponse customResponse = ErrorResponse.of(HttpStatus.NOT_FOUND.value(), "Resource not found");
```
# Checked VS Unchecked Exceptions
- checked -> `intellij` checks it
- [notion notes link](https://calm-individual-12a.notion.site/Spring-246c6b709828819d86a4c6c743109d40)

| Category          | Checked Exception                       | Unchecked Exception                                |
| ----------------- | --------------------------------------- | -------------------------------------------------- |
| **Inherits from** | `java.lang.Exception`                   | `java.lang.RuntimeException`                       |
| **Handling**      | **Required** (try-catch or throws)      | **Optional** (no compile error)                    |
| **Examples**      | `IOException`, `SQLException`           | `NullPointerException`, `IllegalArgumentException` |
| **Common Cause**  | Accessing external resources            | Errors in application logic                        |
| **Design Intent** | To force handling of recoverable errors | To provide flexibility for programmer errors       |
## Checked exception example
```java
public class FileLoader {
    // This method declares that it can throw a checked exception
    public String readFile(String path) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(path));
        return reader.readLine();
    }
}
// calling code
try {
    String result = fileLoader.readFile("test.txt");
} catch (IOException e) {
    // You MUST handle the exception to avoid a compile error
    System.err.println("Failed to read file: " + e.getMessage());
}
```
- ***compiler*** **forces you to handle the exception** because external systems are inherently unpredictable
## Unchecked Exception Example: Internal Validation
```java
public class UserValidator {
    public void validate(String name) {
        if (name == null || name.isBlank()) {
            // Throws an unchecked exception; no 'throws' declaration needed
            throw new IllegalArgumentException("Name is required.");
        }
    }
}
// calling code
// An exception might be thrown here, but a try-catch block is not required
userValidator.validate(null);
```
- the ***developer's responsibility*** to prevent internal logic errors through proper validation beforehand
## Practical Tips & The "Wrap and Rethrow" Strategy
- Use Checked Exceptions at the Lowest Boundary
	- Checked exceptions are best used when directly interacting with external systems (Database, File I/O, Network APIs) where errors are common and often recoverable.
- In service, **catch** the checked exception and **wrap** it in a custom, unchecked business exception before throwing it again
	- This keeps the business logic clean, free of technology-specific error handling, and easier to read
```java
try {
    connection = DriverManager.getConnection(...);
} catch (SQLException e) {
    throw new DatabaseAccessException("DB 연결 실패", e); // → 커스텀 언체크 예외로 래핑
}
```

In service: (conversion)
```java
public void saveUser(User user) {
    try {
        userRepository.save(user); // throws SQLException
    } catch (SQLException e) {
        throw new DatabaseAccessException("회원 저장 실패", e); // 언체크 예외로 래핑
    }
}
```
- Service doesn't have to know about `SQLException`
	- 
- The custom exception is then handled by a global handler (`@ControllerAdvice`).
#### Benefits to doing this

| Reason                          | Explanation                                                                                                                                                  |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Decouples from Technology**   | If technology-specific exceptions (from JDBC, JPA, etc.) propagate to the upper layers, your business logic is no longer technology-agnostic.                |
| **Simplifies Business Logic**   | Overusing `try-catch`  for every possible error clutters the business logic and significantly reduces code readability.                                      |
| **Ensures Consistent Handling** | By converting all exceptions to a custom hierarchy, you can handle them in **one global place** (`@ControllerAdvice`) to create standardized HTTP responses. |
| **Separation of Concerns**      | Low-level external errors are handled at the boundary where they occur. The upper layers can then focus purely on the business domain flow.                  |
- Use unchecked exceptions for Service layers for business logic
# Exception Abstraction - `DataAccessException`
>[!Overview]
>Spring unifies *technology-specific exceptions* (e.g., JDBC’s `SQLException`, JPA’s `PersistenceException`) through an *abstraction layer*: `DataAccessException`
>- Converts them into a consistent set of Spring exceptions so you can handle all data errors with a single strategy

- It's a best practice for your **service layer** to handle only Spring's abstracted exceptions, like `DataAccessException`, instead of technology-specific ones
	- Ensures that if you switch technologies (e.g., from JDBC to JPA), your service-level error handling code doesn't need to change
		- [[Spring Data Access Technologies]]
	- **Technology independence
- When an error occurs, the detailed, technology-specific exception should be logged for debugging purposes, while a clear, abstracted error message is returned to the user or calling layer
## Before
```java
// JDBC에서 발생하는 SQLException 처리 예시
try {
    Connection conn = DriverManager.getConnection(url, username, password);
    // JDBC 작업 수행
} catch (SQLException e) {
    // JDBC 관련 예외 처리 (로직 중복, 복잡도 증가)
}

// JPA에서 발생하는 PersistenceException 처리 예시
try {
    entityManager.persist(entity);
} catch (PersistenceException e) {
    // JPA 관련 예외 처리
}
```
- Issues
	- Code Duplication: You end up writing similar error-handling logic in multiple places for different APIs.
	- Poor Readability: Exception handling code gets mixed in with your business logic, making the code harder to follow.
	- Difficult Maintenance: If you decide to switch from JDBC to JPA, you must go back and rewrite all of your exception handling code.
		- JDBC - [[Spring Data Access Technologies]]
- To prevent this, Spring automatically translates exceptions like `SQLException` into its own `DataAccessException` hierarchy.
	- This decouples your code from the underlying technology, allowing you to write cleaner, more maintainable business logic.
## After
```java
// Spring의 JdbcTemplate을 사용할 경우
try {
    jdbcTemplate.query("SELECT * FROM users", resultSetExtractor);
} catch (DataAccessException e) {
    // 기술에 관계없이 공통된 예외 처리 가능
    log.error("데이터 접근 중 오류 발생", e);
    throw new CustomException("DB 오류 발생");
}
```
## Hierarchy
```
Exception
└── RuntimeException
    └── DataAccessException (Spring의 추상 예외)
        ├── DuplicateKeyException
        ├── EmptyResultDataAccessException
        └── ... (다양한 세부 예외)
```

# Tips
- Instead of catching all exceptions in one giant handler method, create separate, specific `@ExceptionHandler` methods for different exception categories (e.g., one for validation, one for not-found errors).
- Never include the **stack trace** in the JSON response sent to the client. This exposes internal details of your application and is a security risk. Log it on the server, but don't send it.
- Work with the client-side team (frontend or mobile) to define a clear and consistent error code system upfront. This makes integration and collaboration much smoother for everyone.
- For better maintainability, keep your common `ErrorResponse` DTO separate from the DTOs you use for successful responses. Avoid creating a single generic response class that tries to handle both success and error cases.