- [[4 Potential problems in a Spring application#Invalid User Input|Invalid User Input]]
- [[4 Potential problems in a Spring application#Unexpected Exceptions|Unexpected Exceptions]]
- [[4 Potential problems in a Spring application#System Resource Depletion|System Resource Depletion]]
- [[4 Potential problems in a Spring application#External System Integration Failures|External System Integration Failures]]

Summary: Problems & Solutions

| Problem            | Solution                                                                       |
| ------------------ | ------------------------------------------------------------------------------ |
| Invalid Input      | [[Data Transfer Object (DTO)\|DTO]] & Service Validation, [[Bean validation (Input validation)]] |
| Exceptions         | [[Exception handling\|Global Exception Handling]], `Optional`    |
| Resource Depletion | Release Resources, Improve Architecture                                        |
| External Failures  | Timeouts, Retries, Fallbacks                                                   |
| Applies to ALL     | [[Logging]], [[Monitoring]], Standardization (표준화)                             |
# Invalid User Input
- User may make mistake/intentionally submit bad data
- If unchecked this can led to these problems
	- **Compromised Database Integrity**: Storing incorrect or inconsistent data.
	- **System Exceptions**: Causing the application to crash or behave unpredictably.
	- **Business Logic Errors**: Leading to incorrect calculations or process flows.
	- **Security Threats**: Opening vulnerabilities like SQL Injection, Cross-Site Scripting (XSS), etc.
- Examples of Invalid Input
	- **Missing Required Values**: A required email address is `null`.
	- **Incorrect Format**: An email address is missing the '@' symbol.
	- **Out-of-Range Values**: An age is entered as `-1` or `1000`.
	- **Business Rule Violations**: An event's end date is set before its start date.
- Solution 
	- [[Data Transfer Object (DTO)]]
	- [[Bean validation (Input validation)]] - goes in more detail
- Example code
```java
@Data
public class MemberRequestDto {
    @NotBlank(message = "이름은 필수입니다.")
    private String name;

    @Email(message = "올바른 이메일 형식이어야 합니다.")
    private String email;

    @Min(0)
    @Max(150)
    private int age;
}

@PostMapping("/members")
public ResponseEntity register(@RequestBody @Valid MemberRequestDto dto) {
    memberService.register(dto);
    return ResponseEntity.ok().build();
}
```

# Unexpected Exceptions
- Exceptions can occur in situations the developer didn't anticipate. Common examples in Java include:
	- `NullPointerException`
	- `IllegalArgumentException`
	- `IndexOutOfBoundsException`
	- `NoSuchElementException`
- Solution
	- [[Exception handling]] - Use of `ControllerAdvice` and `@ExceptionHandler`
- Example code
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity handleIllegalArgument(IllegalArgumentException e) {
        return ResponseEntity.badRequest().body(e.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity handleException(Exception e) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("서버 내부 오류");
    }
}
```
# System Resource Depletion
- A lack of resources can severely damage system stability. The main resources to monitor are:
	- **DB Connection Pool**: The pool of available database connections.
	- **Memory (Heap/Stack)**: The memory allocated to the application.
	- **Thread Pool**: The available threads to handle requests.
	- **Disk I/O**: The capacity to read from and write to storage.
- Common Causes
	- Failing to release a database connection back to the pool.
	- Infinite loops or infinite recursion in the code.
	- Accumulating massive amounts of data in the cache.
	- Using inefficient algorithms that consume excessive memory or CPU.
- Prevention Methods
	- Use try-with-resources to automatically close resources like DB connections and files.
	- Tune and monitor [[Garbage collection (GC)]] performance. (*not done that much tho*)
	- Limit the maximum number of threads in the thread pool.
		- [[Threads]]
	- Keep the cache size reasonable and implement a proper eviction policy.
	- [[Monitoring]] - ESSENTIAL
```java
try (Connection con = dataSource.getConnection()) {
    // 자동 반환
}
```

# External System Integration Failures
- Modern web services often integrate with various external systems like databases, third-party APIs, and authentication servers. These integrations can fail due to issues on the external system's side or network problems.
- Common Scenarios
	- An external [[APIs|API]] call times out.
	- The service receives a `4xx` (client error) or `5xx` (server error) response.
	- Authentication with an external provider fails.
	- The network connection is lost.
- How to Respond
	- Use [[Logging]] (instead of `sout`)
	- Use [[Monitoring]]
	- **Exception Handling & Retry Logic**: Catch the failure and attempt the call again.
	- **Circuit Breaker Pattern**: Implement a circuit breaker (e.g., using `Resilience4j`) to prevent repeated calls to a failing service.
	- **Fallback Logic**: Provide a default response or alternative functionality when an external system is unavailable.
```java
@CircuitBreaker(name = "externalApi", fallbackMethod = "fallback")
public void callExternalApi() {
    externalApi.call();
}

public void fallback(Throwable t) {
    log.warn("대체 로직 수행");
}
```

