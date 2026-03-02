-  Tips
	- In complex systems, use **correlation IDs (trace IDs)** to analyze relationships between logs effectively.
	- Separate **retention periods and access permissions** for logs according to storage and security policies.
# Good practices
Writing good log messages
- These examples are **calls from your application code** to the logger.
- The messages will get inserted in the placeholder `%msg` in the `logback-spring.xml` file
#### 1. Sufficient context info
```java
// bad
logger.info("User saved successfully");

// good
logger.info("User saved successfully: id={}, username={}, role={}", user.getId(), user.getUsername(), user.getRole());
```
- include `id`, `username`, and `role`
- **Tip:** Key information like `id`, `username`, and `role` should be logged in `key=value` pairs to facilitate searching in tools like ELK, Loki, or Datadog.
#### 2. Maintain Consistent Formatting
```java
logger.info("Event: user_registration, id={}, email={}", id, email);
logger.info("Event: order_created, orderId={}, amount={}", orderId, amount);
logger.info("Event: payment_completed, paymentId={}, method={}", paymentId, method);
```
- Defining and maintaining a consistent log format makes **searching**, **filtering**, and **monitoring** much easier
	- Use ***Key-Value Pairs*** for searchability
- Filtering logs for a specific event like `order_created` is very easy in tools like Kibana or Grafana.
#### 3. Use searchable keywords
```java
logger.info("[USER_CREATE] New user registered: id={}", id);
logger.error("[USER_CREATE] Failed to register new user: id={}", id, e);
logger.info("[ORDER_PROCESS] Order processing started: orderId={}", orderId);
logger.error("[PAYMENT_FAIL] Payment failed: orderId={}, reason={}", orderId, reason);
```
- Tags like `[USER_CREATE]`, `[ORDER_PROCESS]`, `[PAYMENT_FAIL]` inside square brackets serve as event types in log monitoring systems.
#### 4. Parameterized Messages (using `{}`)
```java
// Bad example
// String concatenation happens regardless of the DEBUG level,
// causing unnecessary CPU overhead even if the log won’t be printed.
logger.debug("User ID: " + userId + ", Name: " + userName);

// Good example
logger.debug("User ID: {}, Name: {}", userId, userName);
```
- [[Logging in Spring - Logback#SLF4J|SLF4J]] decides whether to assemble the final message based on the current log level, improving performance.
- Readability also improves, making it easier for team collaboration and code maintenance.
- `{}`
	- It's common to use `{}` for logging. 
	- Adding strings is bad for performance because String is immutable
#### 5. Exclude Sensitive Information
```java
// Bad example
logger.debug("Login attempt: username={}, password={}", username, password);

// Good example
logger.debug("Login attempt: username={}", username);
```
- **Never log sensitive information such as passwords, authentication tokens, social security numbers, or credit card numbers.**
# Logging for Exceptions
- [[Exception handling]]
#### Always include exception object `e`
```java
try {
    // Execute code
} catch (Exception e) {
    logger.error("Error occurred while retrieving user info: userId={}", userId, e);
}
```
#### Choosing Appropriate Log Levels for Exceptions (log lvls)
- Treating all exceptions as `ERROR` can cause excessive alerts and dilute the significance of genuine errors. 
- Differentiate log levels like `WARN` and `ERROR` based on the severity and recoverability of the exception.
```java
try {
    response = apiClient.callExternalApi(request);
} catch (NotFoundException e) {
    logger.warn("Requested resource not found: resourceId={}", resourceId, e);
    return ResponseEntity.notFound().build();
} catch (AuthenticationException e) {
    logger.warn("API authentication failed: apiKey={}", apiKey.substring(0, 5) + "...", e);
    return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
} catch (Exception e) {
    logger.error("Unexpected error during API call: request={}", request, e);
    return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
}
```
#### Include Contextual Information in Exception Logs
```java
@ExceptionHandler(OrderProcessingException.class)
public ResponseEntity<ErrorResponse> handleOrderProcessingException(OrderProcessingException e, HttpServletRequest request) {
    logger.error("주문 처리 예외: orderId={}, status={}, items={}, paymentMethod={}, clientIP={}, requestURL={}",
        e.getOrderId(),
        e.getOrderStatus(),
        e.getItems().size(),
        e.getPaymentMethod(),
        request.getRemoteAddr(),
        request.getRequestURL(),
        e);

    ErrorResponse response = new ErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR.value(), e.getMessage());
    return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(response);
}
```
- log the **key state information (context)** at the time the exception occurs

# Examples for projects
- Goals
	- Identify and understand key points where logs are recorded.
	- Learn practical methods for searching and analyzing logs.
	- Develop skills to reproduce and resolve issues based on logs.
- These are all good examples on where we can put logs!
## Application start/end
- Logging the start and stop points during an application’s lifecycle greatly aids debugging in case of failures or abnormal shutdowns.
- [[@SpringBootApplication]]

```java
// Spring Boot 애플리케이션의 시작 시점 로깅
@SpringBootApplication
public class MyApplication {
    private static final Logger logger = LoggerFactory.getLogger(MyApplication.class);

    public static void main(String[] args) {
        logger.info("애플리케이션 시작 중..."); // 애플리케이션 부팅 시작 시점
        SpringApplication.run(MyApplication.class, args);
        logger.info("애플리케이션이 성공적으로 시작됨"); // 부팅 완료 시점
    }
}

// 종료 시점 로깅 - @PreDestroy를 이용한 종료 훅
@Component
public class ApplicationShutdownHandler {
    private static final Logger logger = LoggerFactory.getLogger(ApplicationShutdownHandler.class);

    @PreDestroy
    public void onShutdown() {
        logger.info("애플리케이션 종료 중...");
    }
}
```

## Request start/end
- In HTTP-based applications, logging at the **per-request level** is crucial. 
- Below is an example using a **filter** to log the start and end of each request.
```java
@Component
public class RequestLoggingFilter extends OncePerRequestFilter {
    private static final Logger logger = LoggerFactory.getLogger(RequestLoggingFilter.class);

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        long startTime = System.currentTimeMillis();
        String requestURI = request.getRequestURI();
        String method = request.getMethod();

        logger.info("요청 시작: {} {}", method, requestURI); // 요청 시작 로그

        try {
            filterChain.doFilter(request, response); // 실제 요청 처리
        } finally {
            long duration = System.currentTimeMillis() - startTime;
            int status = response.getStatus();

            // 상태 코드에 따라 로그 레벨을 다르게 설정
            if (status >= 400) {
                logger.warn("요청 종료: {} {} - 상태 코드: {}, 처리 시간: {}ms", method, requestURI, status, duration);
            } else {
                logger.info("요청 종료: {} {} - 상태 코드: {}, 처리 시간: {}ms", method, requestURI, status, duration);
            }
        }
    }
}
```
## Business logic
- In core domain processes like order handling, user registration, and payment, add logging at appropriate points to enable flow tracing and exception debugging.
```java
@Service
public class OrderService {
    private static final Logger logger = LoggerFactory.getLogger(OrderService.class);

    @Transactional
    public OrderResult processOrder(OrderRequest orderRequest) {
        logger.info("주문 처리 시작: requestId={}, 상품 수={}",
                   orderRequest.getRequestId(), orderRequest.getItems().size());

        // 1. 재고 확인
        boolean stockAvailable = checkStock(orderRequest.getItems());
        if (!stockAvailable) {
            logger.warn("재고 부족으로 주문 실패: requestId={}", orderRequest.getRequestId());
            throw new InsufficientStockException("재고가 부족합니다");
        }

        // 2. 결제 처리
        logger.info("결제 처리 중: requestId={}, 금액={}",
                   orderRequest.getRequestId(), orderRequest.getTotalAmount());
        PaymentResult paymentResult = paymentService.processPayment(orderRequest.getPayment());

        // 3. 주문 생성
        Order order = createOrder(orderRequest, paymentResult);
        logger.info("주문 생성 완료: orderId={}, 상태={}",
                   order.getId(), order.getStatus());

        return new OrderResult(order.getId(), order.getStatus());
    }
}
```
## External System Integration
Logging is essential for identifying performance bottlenecks when interacting with external resources such as database queries, API calls, and Redis access.
```java
@Repository
public class ProductRepository {
    private static final Logger logger = LoggerFactory.getLogger(ProductRepository.class);

    @PersistenceContext
    private EntityManager entityManager;

    public List<Product> findProductsByCategory(String category, int limit) {
        String jpql = "SELECT p FROM Product p WHERE p.category = :category ORDER BY p.popularity DESC";

        logger.debug("상품 조회 JPQL 실행: category={}, limit={}, query={}",
                    category, limit, jpql); // 쿼리 실행 전 로그

        long startTime = System.currentTimeMillis();
        List<Product> products = entityManager.createQuery(jpql, Product.class)
                                           .setParameter("category", category)
                                           .setMaxResults(limit)
                                           .getResultList();
        long duration = System.currentTimeMillis() - startTime;

        logger.debug("상품 조회 완료: category={}, 결과 수={}, 소요시간={}ms",
                    category, products.size(), duration);

        if (duration > 1000) {
            logger.warn("상품 조회 쿼리 실행 시간 지연: category={}, 소요시간={}ms",
                       category, duration);
        }

        return products;
    }
}
```
# Tips
- You must log **failures**. 
- Excessive logging inside loops for debugging can actually *degrade performance*.
- You have to include 'e' when writing an exception. Even if there's nowhere to receive it, the logging library will record it
	- you must pass the exception object `e` as the **last argument** to your logging call
	- `log.error("Error processing request for userId={}", userId, e);`