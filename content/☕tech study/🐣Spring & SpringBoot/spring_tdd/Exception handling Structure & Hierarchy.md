- [[Exception handling]]
# In a [[⭐Layered Architecture|layered architecture]]
```
src/main/java
└── com/example/myapp/
    ├── controller
    ├── service
    ├── repository
    └── exception
```
- Layers
	- **Controller**: Receives client requests and calls the appropriate service.
	- **Service**: Contains the core business logic.
	- **Repository**: Manages data access and interaction with the database.
	- **Exception**: A dedicated package for custom exception classes and global handlers.
- In modern Spring applications, *the best practice is to manage exceptions centrally at the **controller layer***.
	- Typically done using a combination of **`@ControllerAdvice`**, **`@ExceptionHandler`**, and custom exception classes.
# Exception hierarchy
>[!Overview]
>A structured system of custom exception classes designed to represent various error scenarios in your application in a meaningful way. 
>- Like creating a well-organized family tree for all possible errors.

- Designing a *clear hierarchy* provides several key benefits:
	- **Improved Maintainability**: By grouping related exceptions, you can create consistent handling logic.
	- **Separation of Responsibilities**: You can define exceptions specific to a domain, layer, or feature, which clarifies where an error originates.
	- **Better User Experience**: Meaningful error codes and messages help users and developers understand exactly what went wrong.
# Basic Design (components)
- This is the basic design of an exception hierarchy => shows **WHAT components you need**
	- this step is only about **defining the exceptions themselves**—the things that will be _thrown_ (the `GlobalExceptionHandler` is not here)
- A typical design involves a dedicated `exception` package with a few core components:
```
src/main/java/com/example/app/
└── exception/
    ├── BaseException.java              # The parent for all custom exceptions
    ├── ErrorCode.java                  # An enum defining all error codes and messages
    ├── ValidationException.java        # Used for input validation failures
    └── ResourceNotFoundException.java  # Used when a requested resource isn't found
```
- Even if your application is small at first, establishing this exception structure from the beginning will *make it much easier to scale and maintain as the project grows*
#### `BaseException.java`
```java
// 모든 커스텀 예외가 상속받는 추상 클래스입니다.
public abstract class BaseException extends RuntimeException {
    private final ErrorCode errorCode;

    public BaseException(ErrorCode errorCode) {
        super(errorCode.getMessage());
        this.errorCode = errorCode;
    }

    public ErrorCode getErrorCode() {
        return errorCode;
    }
}
```
#### `Errorcode.java` (notion)
```java
// 예외 코드와 메시지를 Enum으로 관리합니다.
public enum ErrorCode {
    INVALID_INPUT("E001", "입력값이 유효하지 않습니다."),
    USER_NOT_FOUND("E002", "사용자를 찾을 수 없습니다."),
    PERMISSION_DENIED("E003", "권한이 없습니다.");

    private final String code;
    private final String message;

    ErrorCode(String code, String message) {
        this.code = code;
        this.message = message;
    }

    public String getCode() {
        return code;
    }

    public String getMessage() {
        return message;
    }
}
```
- It's a great practice to include the relevant **`HttpStatus`** directly within your `ErrorCode` enum or `BaseException` class
- [[Systematizing Error Codes]]
#### `Errorcode.java` 🏆(gemini)
```java
import org.springframework.http.HttpStatus;

public enum ErrorCode {
    // Defines the HTTP Status, our unique code, and the message
    USER_NOT_FOUND(HttpStatus.NOT_FOUND, "U001", "The requested user was not found."),
    INVALID_AUTH_TOKEN(HttpStatus.UNAUTHORIZED, "A002", "The provided auth token is invalid."),
    DUPLICATE_EMAIL(HttpStatus.CONFLICT, "U002", "This email is already in use.");

    private final HttpStatus status;
    private final String code;
    private final String message;

    ErrorCode(HttpStatus status, String code, String message) {
        this.status = status;
        this.code = code;
        this.message = message;
    }

    // Getters...
}
```

#### `ValidationException.java`
```java
// 입력값 검증 실패에 대한 예외입니다.
public class ValidationException extends BaseException {
    public ValidationException(ErrorCode errorCode) {
        super(errorCode);
    }
}
```

#### `ResourceNotfoundException.java`
```java
// 리소스를 찾을 수 없는 경우 발생하는 예외입니다.
public class ResourceNotFoundException extends BaseException {
    public ResourceNotFoundException(ErrorCode errorCode) {
        super(errorCode);
    }
}
```

# Separating Exceptions by Domain
- ***Once you have your building blocks from above (basic design components), you need to decide where to put the files.***
- **Strategy for organizing the components by business domain**
	- the recommended approach is to separate exception classes by their functional domain
- Structuring your exceptions by domain from the start makes it much easier to **reuse** this logic if you later decide to use [[Microservice Architecture (MSA)|microservices]].
```
src/
└── exception/
    ├── common/
    │   └── BaseException.java      // Parent for all exceptions
    ├── user/
    │   ├── UserNotFoundException.java
    │   └── DuplicateEmailException.java
    └── auth/
        ├── TokenExpiredException.java
        └── UnauthorizedAccessException.java
```

Examples
```java
public class UserNotFoundException extends BaseException {
    public UserNotFoundException() {
        super(ErrorCode.USER_NOT_FOUND);
    }
}
```

| Aspect                  | Benefit / Description                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| **Separation Criteria** | The Business Domain (e.g., User, Auth, Order).                                                   |
| **Advantages**          | Clear separation of responsibilities, prevents merge conflicts, and simplifies testing.          |
| **Scalability**         | This structure is easy to reuse if you later transition to a [[Microservice Architecture (MSA)]] |
# Structure with `GlobalExceptionHandler`
- Now that you've defined what your exceptions look like (Step 1) and organized where they live (Step 2), you need to create the mechanism that **catches** them when they're thrown => ``GlobalExceptionHandler``
- [[Exception handling#Global Exception Handling -`GlobalExceptionHandler`|Exception handling: Global Exception Handling -GlobalExceptionHandler]]

**All-in-One Exception Package**
```
src
├── common
│   └── exception
│       ├── GlobalExceptionHandler.java       // @RestControllerAdvice 적용
│       ├── ErrorResponse.java                // 클라이언트 응답용 DTO
│       ├── ErrorCode.java                    // 에러 코드 enum
│       └── CustomException.java              // 기본 예외 클래스
```

**Separated by Role/Layer (Better Practice)** ✨
```
src
├── common
│   ├── advice
│   │   └── GlobalExceptionHandler.java       // @RestControllerAdvice 적용
│   ├── exception
│   │   ├── ErrorCode.java                    // 에러 코드 enum
│   │   └── CustomException.java              // 기본 예외 클래스
│   ├── response
│   │   └── ErrorResponse.java                // 클라이언트 응답용 DTO
```

In a real world project, you would combine the "Separating Exceptions by Domain" with above: (gemini)
```
src/main/java/com/example/app/
├── common/                               // Shared modules for the whole application
│   ├── advice/
│   │   └── GlobalExceptionHandler.java   // << STEP 3: The Catcher
│   ├── response/
│   │   └── ErrorResponse.java            // << STEP 1: A Building Block
│   └── exception/                        // STEP 2: Organize Domain Exceptions
│       ├── BaseException.java            // << STEP 1: A Building Block
│       └── ErrorCode.java                // << STEP 1: A Building Block
│
├── user/                                 // 'User' domain package
│   ├── controller/
│   ├── service/
│   └── exception/                        // STEP 2: Organize Domain Exceptions
│       └── UserNotFoundException.java    // << STEP 1: A Building Block
│       └── DuplicateEmailException.java  // << STEP 1: A Building Block
│
└── auth/                                 // 'Auth' domain package
    ├── ...
    └── exception/                        // STEP 2: Organize Domain Exceptions
        └── TokenExpiredException.java    // << STEP 1: A Building Block
```