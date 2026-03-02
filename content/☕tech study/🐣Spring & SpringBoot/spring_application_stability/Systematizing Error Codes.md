
>[!Overview]
>Relying on text messages alone is not enough for users, developers, and log analysis systems to quickly and accurately identify an error. 
>- Using a *coded error identifier* allows for *consistent and automated responses* to any given problem.

- [[Exception handling Structure & Hierarchy#`Errorcode.java`|Exception handling Structure & Hierarchy - Errorcode.java]]

# Designing the Error Code System
- A common and effective approach is to create a structured code that combines a **domain prefix** with a **unique number**. 
- For example:
	- **USR**: User-related errors
	- **AUT**: Authentication-related errors
	- **VAL**: Input validation errors
- different methods
	- This system is typically implemented using a Java **`enum`**, which serves as a single source of truth for all possible errors in your application
	- You can use external sources too
		- `.yml` file
		- database
#### Using `enum` (most common)
```java
public enum ErrorCode {
    // --- User Errors ---
    USER_NOT_FOUND("USR_001", "The requested user was not found."),

    // --- Authentication Errors ---
    AUTHENTICATION_FAILED("AUT_002", "Authentication has failed."),

    // --- Validation Errors ---
    INVALID_INPUT_VALUE("VAL_003", "The provided input value is invalid.");

    private final String code;
    private final String message;

    ErrorCode(String code, String message) {
        this.code = code;
        this.message = message;
    }

    // Getters...
}
```

(how its called in other code for example)
```java
public class BusinessException extends RuntimeException {
    private final ErrorCode errorCode;

    public BusinessException(ErrorCode errorCode) {
        super(errorCode.getMessage()); // Set the exception message
        this.errorCode = errorCode;
    }

    public ErrorCode getErrorCode() {
        return errorCode;
    }
}
```

```java
@Service
public class UserService {
    public User findUserById(Long id) {
        // ... logic to find a user from the database
        User user = userRepository.findById(id).orElse(null);

        if (user == null) {
            // Throw the exception using the enum constant
            throw new BusinessException(ErrorCode.USER_NOT_FOUND);
        }
        return user;
    }
}
```

#### `yaml` file
```java
errors:
  USER_NOT_FOUND:
    code: "USR_001"
    message: "The requested user was not found."
    httpStatus: 404
  INVALID_INPUT_VALUE:
    code: "VAL_003"
    message: "The provided input value is invalid."
    httpStatus: 400
```

#### database
You'd have a table named something like `error_codes`.

|`error_key`|`error_code`|`message`|`http_status`|
|---|---|---|---|
|`USER_NOT_FOUND`|`USR_001`|The user was not found.|404|
|`INVALID_INPUT`|`VAL_003`|Invalid input value.|400|
# Practical Tips
- **Frontend Integration**
	- Error codes are incredibly useful for [[Frontend|frontend development]]. 
	- The [[Backend & Main Components Explained#Client-side vs Server-side|client-side application]] can use a specific code (e.g., `AUT_002`) to trigger a specific action, like redirecting the user to a login page.
- **Log Analysis**
	- In log collection systems like **Sentry**, **Datadog**, or the **ELK Stack**, you can easily filter and create alerts based on specific error codes
	- Searching for a code like `USR_001` is far more reliable than searching for a text message that might change.