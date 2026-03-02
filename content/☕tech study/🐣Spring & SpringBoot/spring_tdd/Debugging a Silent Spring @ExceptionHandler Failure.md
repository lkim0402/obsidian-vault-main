```java
@DisplayName("유저 삭제 테스트 실패 - 잘못된 요정")  
@Test  
void deleteUser_userNotFound() throws Exception {  
  // ================== given ==================  
  UUID userId = UUID.randomUUID();  
  doThrow(new UserNotFoundException(userId))  
      .when(userService)  
      .delete(userId);  
  
  // ================== when & then ==================  
  mockMvc.perform(delete("/api/users/" + userId))  
      .andExpect(status().isNotFound());  
}
```
- The core problem
	- A `MockMvc` test, designed to verify a `404 Not Found` status for a delete operation on a non-existent user, was unexpectedly failing with a `500 Internal Server Error`.
	- The issue wasn't in the test logic or the controller. The root cause was a **silent crash inside the `@ExceptionHandler` method itself**, which prevented it from creating the correct `404` error response. This internal crash was triggered by a chain of seemingly unrelated issues.

## Root cause
The fundamental issue was in our base custom exception, `DiscodeitException`. Its constructor received an error `message` but **never passed it up to the parent `RuntimeException` class**. The call to `super(message)` was missing.

#### Incorrect code:
```java
// DiscodeitException.java
@Getter
public abstract class DiscodeitException extends RuntimeException {
    // ... other fields
    
    public DiscodeitException(String message, /*...other params...*/) {
        // PROBLEM: The message is never sent to the parent RuntimeException.
        // super(message); <-- THIS LINE WAS MISSING.
        // ...
    }
}
```
- All exceptions like `UserException` (which has child exceptions like `UserNotFoundException`) extends `DiscodeitException`
- In `DiscodeitException` the `super` was missing
	- Because of this, any attempt to call `e.getMessage()` on an instance of `DiscodeitException` (or its subclasses) would return `null`.

Handler Code
```java
// GlobalExceptionHandler.java
@Slf4j  
@RestControllerAdvice  
public class GlobalExceptionHandler {  
  
  @ExceptionHandler(DiscodeitException.class)  
  public ResponseEntity<ErrorResponse> handleException(DiscodeitException e) {  
    log.warn("Custom exception handled [{}]: {}", e.getErrorCode(), e.getMessage()); // <<<<<<< HERE
    ErrorResponse errorResponse = new ErrorResponse(e);  
    return new ResponseEntity<>(errorResponse, e.getErrorCode().getStatus());  
  }
  ...
}
```
- So, when `e.getMessage()` returned `null`, the logging framework threw a `NullPointerException`. This new, unexpected exception crashed the `handleException` method mid-execution.

 (`UserNotFoundException` and `UserException` that was being used, which is correct)
```java
public class UserNotFoundException extends UserException {  
  
  public UserNotFoundException(UUID userId) {  
    super(  
        Instant.now(),  
        ErrorCode.USER_NOT_FOUND,  
        Map.of("userId", userId));  
  }  
}
```

```java
public class UserException extends DiscodeitException {  
  
  public UserException(Instant timestamp, ErrorCode errorCode, Map<String, Object> details) {  
    super(errorCode.getMessage(), timestamp, errorCode, details);  
  }  
}
```

#### Corrected code
```java
@Getter  
public abstract class DiscodeitException extends RuntimeException {  
  
  private final Instant timestamp;  
  private final ErrorCode errorCode;  
  private final Map<String, Object> details;  
  
  // The 'message' parameter is passed to the parent 'RuntimeException'  
  public DiscodeitException(String message, Instant timestamp, ErrorCode errorCode,Map<String, Object> details) {  
    super(message); // this is the fix  
    this.timestamp = timestamp;  
    this.errorCode = errorCode;  
    this.details = details != null ? Map.copyOf(details) : Map.of();  
  }  
}
```
- You need to add `super(message)`

GlobalHandler
```java
@Slf4j  
@RestControllerAdvice  
public class GlobalExceptionHandler {  
  
  @ExceptionHandler(DiscodeitException.class)  
  public ResponseEntity<ErrorResponse> handleException(DiscodeitException e) {  
    log.warn("Custom exception handled [{}]: {}", e.getErrorCode(), e.getMessage());
    ErrorResponse errorResponse = new ErrorResponse(e);  
    return new ResponseEntity<>(errorResponse, e.getErrorCode().getStatus());  
  }
  ...
}
```
- Now when `e.getMessage` is called, we don't get the error!

```java
@Getter  
@AllArgsConstructor // <-- This annotation creates the constructor you need  
public class ErrorResponse {  
  
  private final Instant timestamp = Instant.now();  
  private final String code;  
  private final String message;  
  private final Map<String, Object> details;  
  private final String exceptionType; // 발생한 예외의 클래스 이름  
  private final int status; // HTTP 상태코드  
  
  // custom ErrorResponse  
  public ErrorResponse(DiscodeitException e) {  
    this.code = e.getErrorCode().getCode();  
    this.message = e.getErrorCode().getMessage();  
    this.details = e.getDetails();  
    this.exceptionType = e.getClass().getSimpleName();  
    this.status = e.getErrorCode().getStatus().value();  
  }  
  
}
```