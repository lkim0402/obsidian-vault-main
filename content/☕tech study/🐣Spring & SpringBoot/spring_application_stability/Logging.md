
>[!Overview]
>**Logging** is the act of recording and storing information about events that occur while an application is running. 
>- Unlike a live step-debugger, logs serve as a historical record for investigation
>- Essential for understanding why an exception occurred, analyzing system behavior, and conducting security audits.

- A ***log***
	- **A text-based record of various events that occur during the execution of an application**
- When a problem arises your logging must be able to answer these
	- When did the event happen and what request triggered it?
	- What specific error occurred?
	- What were the input values for the operation?
	- What path did the code take to execute the logic?
- **NEVER LOF SENSITIVE INFORMATION**
- Log also when there's a failure
- A typical log entry includes a **timestamp**, **log level** (e.g., INFO, ERROR), the **message**, and contextual information (like the class or thread name).
- [[Logging in Spring - Logback]] (and SLF4J)
	- [[Writing Logging Messages]]
	- [[Log Search & Analyzing]]
- [[Exception handling]]
# Example directory
```
프로젝트 루트 구조 예시

project-root/
├── logs/                  # 로그 파일 저장 디렉토리
│   └── app.log
├── src/
│   └── ...
└── application.properties # 로그 설정 파일 포함 가능
```

`app.log` is the actual log file where your application’s runtime log messages are written:
```
2025-08-09 12:34:56.789 [main] INFO  com.example.myapp.Service - Application started successfully.
2025-08-09 12:35:01.123 [http-nio-8080-exec-1] DEBUG com.example.myapp.Controller - Received request for /api/data
2025-08-09 12:35:05.456 [main] ERROR com.example.myapp.Repository - Database connection failed
```
# Reasons

|Purpose|Description|
|---|---|
|**Understanding Application Behavior**|You can monitor and understand how the system is processing requests and data in real-time.|
|**Problem Analysis**|When an exception occurs, logs allow you to trace the sequence of events and find the root cause of the failure.|
|**Tracking Business Events**|You can record important user actions, like logins or order placements, to gather business intelligence and statistics.|
|**Security Auditing**|Logs create an audit trail, allowing you to track who accessed what data and when, which is crucial for security and compliance.|
# Tech stack - ELK Stack
>[!Overview]
>A very popular set of tools for centrally collecting, searching, and visualizing log data from all your servers
>- Used primarily for logging, but also powerful for [[Monitoring]]

- E = Elasticsearch
    - **The Search Engine & Database.** 
	- Elasticsearch takes the structured logs from Logstash and stores them.
	- Crucially, it's a highly optimized search engine (like Google, but for your own data). As your note says, when you have **TONS of logs**, you need a powerful tool like Elasticsearch to search through them instantly to find specific errors or information.
	- Elasticsearch is also used a lot by itself, or connected to many other things like github or wikipedia
- L = Logstash
    - **The Collector.** 
    - Logstash is a data processing pipeline that sits on your servers. Its job is to collect logs from various sources (like application files), *parse and filter them into a structured format* (like JSON), and then *send them to a central location*.
- K = Kibana (키바나)
    - **The Dashboard.** 
    - Kibana is a web interface with *powerful visualization tools*. It connects to Elasticsearch and lets you explore your logs by creating charts, graphs, maps, and dashboards. 
    - This helps you spot trends, identify errors, and monitor the health of your application visually.

# Example Use Cases for Logging
## Understanding Application Behavior
```java
@Service
public class UserService {
    private static final Logger logger = LoggerFactory.getLogger(UserService.class);

    public User getUserById(Long id) {
        logger.debug("getUserById 메서드 호출: id={}", id);

        // 메서드 실행 시간 체크
        long startTime = System.currentTimeMillis();
        User user = userRepository.findById(id).orElse(null);
        long endTime = System.currentTimeMillis();

        if (user != null) {
            logger.debug("사용자 조회 완료: {}, 소용시간: {}ms", user.getUsername(), (endTime - startTime));
        } else {
            logger.warn("ID가 {} 인 사용자를 찾을 수 없음", id);
        }

        return user;
    }
}
```
## Problem analysis
```java
@Controller
public class ProductController {
    private static final Logger logger = LoggerFactory.getLogger(ProductController.class);

    @GetMapping("/products/{id}")
    public String getProduct(@PathVariable Long id, Model model) {
        try {
            Product product = productService.getProductById(id);
            model.addAttribute("product", product);
            logger.info("제품 페이지 조회 성공: id={}, name={}", id, product.getName());
            return "product/detail";
        } catch (ProductNotFoundException e) {
            logger.warn("존재하지 않는 제품 조회 시도: id={}", id, e);
            return "product/not-found";
        } catch (Exception e) {
            logger.error("제품 조회 중 예금치 않은 오류 발생: id={}", id, e);
            return "error/500";
        }
    }
}
```

## Tracking and Recording Business Events
- This data can be used to generate statistics and provide valuable business intelligence
```java
@Service
public class OrderService {
    private static final Logger logger = LoggerFactory.getLogger(OrderService.class);

    @Transactional
    public Order placeOrder(OrderRequest orderRequest, User user) {
        logger.info("주문 시작: 사용자={}, 상품 수={}, 총액={}",
			user.getUsername(),
			orderRequest.getItems().size(),
			orderRequest.getTotalAmount());

        Order order = new Order();
        // ... 주문 처리 로직 ...

        logger.info("주문 완료: 주문번호={}, 사용자={}, 결제방법={}",
			order.getOrderNumber(),
			user.getUsername(),
			order.getPaymentMethod());

        return order;
    }
}
```

## Security Auditing
```java
@Service
public class UserSecurityService {
    private static final Logger securityLogger = LoggerFactory.getLogger("SECURITY_AUDIT");

    public void login(String username, String ipAddress) {
        securityLogger.info("사용자 로그인: username={}, ip={}, time={}",
			   username, ipAddress, LocalDateTime.now());
    }

    public void accessSensitiveData(User user, String dataType) {
        securityLogger.info("민감 데이터 접근: username={}, dataType={}, time={}",
			   user.getUsername(), dataType, LocalDateTime.now());
    }

    public void changeUserRole(User admin, User targetUser, String newRole) {
        securityLogger.info("사용자 권한 변경: admin={}, targetUser={}, newRole={}, time={}",
			   admin.getUsername(), targetUser.getUsername(), newRole, LocalDateTime.now());
    }
}
```
