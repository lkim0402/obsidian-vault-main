- [[Logging]]
- [[Configuration yaml settings for logging + testing]]
# SLF4J & Logback
## SLF4J
>[!Overview]
>- SLF4J is not a logging framework itself; it's an **abstraction layer**, or a **facade**, that sits on top of other logging frameworks like Logback or Log4j2. 
>- Your application code uses the standard SLF4J API to write logs, and a separate, *underlying framework (the "implementation") is responsible for the actual output*, like *Logback*

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ExampleClass {
    // Get a logger instance using the SLF4J factory
    private static final Logger logger = LoggerFactory.getLogger(ExampleClass.class);

    public void doSomething() {
        logger.debug("This is a debug message.");
        logger.info("This is an info message.");
        logger.warn("This is a warning message.");
        logger.error("This is an error message.");
    }
}
```
## Logging Implementations (What you plug into SLF4J)
>The actual logging framework implementations
- Logback
	- The native and *most recommended* implementation for the SLF4J API
	- The **default** logging implementation included with `spring-boot-starter-web`.
	- fast, efficient, and packed with features:
		- **High performance** and low memory footprint.
		- **Automatic reloading** of its configuration file without restarting the server.
		- **Log file rolling** based on time (e.g., daily) or file size.
		- Advanced options like **conditional filtering** and highly customizable output patterns.
- Log4j / Log4j2
	- **Log4j** is an older, widely used framework.
	- original Log4j had a **major security vulnerability** (Log4Shell), which caused many to migrate away from it
	- Log4j2 is the rewritten, modern version.... but ppl use Logback more and more
- We just use it without having to know the implementation -> you can switch the underlying logging framework from Logback to Log4j2 without changing a single line of your application code
## The Relationship Between SLF4J and Logback
The flow is simple: your code only talks to SLF4J.
```
Application Code ➡️ SLF4J API ➡️ Logback (Implementation) ➡️ Log Output (File, Console, etc.)
```
- **SLF4J** provides the standard **interface** (`Logger`). It defines _how_ you write a log message.
- **Logback** provides the **implementation**. It's the engine that _actually processes_ the message and writes it to its destination.
- Because your code only depends on SLF4J, you can *switch out the underlying logging framework (e.g., from Logback to Log4j2) without changing a single line of your application code*.
## Spring Boot uses Logback
- By default, Spring Boot uses **Logback** for logging, which is included automatically through the `spring-boot-starter-logging` dependency. 
	- It's already in `spring-boot-starter-web`
	- [[Basic Dependencies]]

Using `application.properties`
```yaml
logging:
  level:
    root: INFO
    com.example.myapp: DEBUG
  file:
    name: myapp.log
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
```

- If we want more detailed settings, we use `logback-spring.xml`, essentially the configuration file where you define
	- **Appenders** → where the logs go (console, file, rolling file, etc.)
	- **Encoders** → how the logs are formatted
	- **Loggers** → which packages/classes to log and at what level
	- **Root logger** → default logging level and appenders

# Log Levels
- Using standard log levels helps you filter and understand the state of your application more effectively.
	- [[4 Potential problems in a Spring application]]
- Tips
	- Activate `TRACE` and `DEBUG` levels only in development or local environments; in production, log only `INFO` level and above.
	- Always log exceptions (`Throwable` objects) with `ERROR` messages to capture stack traces.
	- Treat `WARN` logs as signals for long-term improvements, even if they don't indicate immediate service failures.
- In real world applications `WARN`, `ERROR` are most used

| Level     | Purpose                                                                                                                                              | Examples                                                                                                                                            |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ERROR** | Indicates a ***critical* system failure** that prevents normal operation.                                                                            | - DB connection failure<br>- `NullPointerException`<br>- out of memory errors<br>- external system errors                                           |
| **WARN**  | Highlights a **potentially harmful situation** that requires attention, though the application can still function.                                   | - Failed login attempt<br>- invalid user input<br>- performance degradation<br>- nearing limits<br>- Other potential issues                         |
| **INFO**  | Records the **normal flow of the application** and major milestones.<br>-used in production environments as well                                     | - User registration successful<br>- order completed<br>- service started                                                                            |
| **DEBUG** | Provides **detailed information** useful only during *development and testing*<br>- provides detailed info when they're debugging a specific problem | - Internal variable states<br>- detailed logic flow<br>- API request/response bodies<br>- method calls, important variable values, SQL queries, etc |
| **TRACE** | Offers the **most granular level of detail**, tracing the code's execution path. <br>- Typically used only in development (rarely in production)     | - method entry/exit<br>- variable changes<br>-  loop iterations                                                                                     |
- Setting the minimum log level for different parts of your application
	- tells your logging framework (Logback) which messages are important enough to show and which ones to ignore
	- ex. Any class in `com.example.myapp.controller`, the configuration is `DEBUG`, so you will see logs at these levels: `DEBUG`, `INFO`, `WARN`, `ERROR`
```YAML
logging:
  level:
    root: INFO
    com.example.myapp.controller: DEBUG
    org.hibernate: WARN
```
## Example of log output by level (long)
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

public class LoggingDemo {

    // Logger 인스턴스 생성 (현재 클래스 기준)
    private static final Logger logger = LoggerFactory.getLogger(LoggingDemo.class);

    // 예시 변수 (실제 사용 시 적절히 초기화되어야 함)
    private int amount;
    private User user;
    private List<Item> items;
    private long startTime;
    private long endTime;
    private String appVersion;
    private String username;
    private String orderId;
    private String apiName;
    private long executionTime;
    private String query;
    private String missingParam;
    private String userId;
    private ResponseEntity<String> response;

    public void process() {

        // TRACE: 메서드 진입, 반복문 흐름 등 상세한 실행 흐름을 추적할 때 사용
        logger.trace("payment 메서드 시작: amount={}, user={}", amount, user.getId());

        for (int i = 0; i < items.size(); i++) {
            // TRACE: 반복문 내부의 아이템 처리 상태 추적
            logger.trace("item[{}] 처리 중: {}", i, items.get(i).getName());
        }

        logger.trace("payment 메서드 종료: result={}", true); // 처리 결과를 추적하는 로그

        // DEBUG: 개발 중 상태 확인용 로그 (운영 환경에서는 출력 X)
        logger.debug("사용자 조회 쿼리 실행: id={}", userId);
        logger.debug("조회된 사용자 정보: {}", user);
        logger.debug("처리 시간: {}ms", (endTime - startTime));

        // INFO: 주요 기능의 정상 수행 정보 (운영 환경에서 출력)
        logger.info("애플리케이션 시작: 버전={}", appVersion);
        logger.info("사용자 로그인 성공: username={}", username);
        logger.info("주문 처리 완료: orderId={}, amount={}", orderId, amount);

        // WARN: 시스템은 계속 작동하지만, 주의가 필요한 상황
        logger.warn("사용 중단 예정 API 호출: {}", apiName);
        logger.warn("느린 쿼리 실행: {}ms, query={}", executionTime, query);
        logger.warn("필수가 아닌 파라미터 누락: {}", missingParam);

        // ERROR: 예외 발생 시 - 반드시 예외 객체 포함하여 스택트레이스 로그 출력
        try {
            // 예외 발생 가능 코드 블록
        } catch (Exception e) {
            logger.error("주문 처리 중 오류 발생: orderId={}", orderId, e);
        }

        // 조건 분기에서 API 호출 실패 처리
        if (response.getStatusCode() != HttpStatus.OK) {
            // ERROR: 외부 시스템 또는 내부 로직에서 실패 상황 기록
            logger.error("API 호출 실패: status={}, message={}",
                         response.getStatusCode(),
                         response.getBody());
        }
    }
}
```
# Log Output Pattern (syntax) + `logback-spring.xml`
The pattern format for making log messages more readable consists of the following elements:

|Pattern|Meaning|
|---|---|
|`%d`|Date/Time|
|`%thread`|Thread name|
|`%-5level`|Log level (fixed to 5 characters)|
|`%logger{36}`|Logger name (up to 36 characters)|
|`%msg`|Message content|
|`%n`|Newline|
- **Logback configuration** written in **XML**
	- the default logging framework Spring Boot uses (unless you switch it)
- This file is `logback-spring.xml` 
	- [[Configuration yaml settings for logging + testing#`logback-spring.yml`|You can also use yml but its limited]]
```xml
<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```
Example output
```
2023-05-15 14:23:45.678 [http-nio-8080-exec-1] INFO c.e.m.controller.UserController - 사용자 조회: id=123
```

# Log file management (policies)
- If log files are left unmanaged in a production environment, they can *quickly consume disk space*. 
- Therefore, it is important to apply the following policies:

| Strategy Item         | Description                                                                      |
| --------------------- | -------------------------------------------------------------------------------- |
| `maxHistory`          | Sets the number of days to retain logs. <br>- Example: 90 days                   |
| `totalSizeCap`        | Limits the total size of logs. <br>- When exceeded, older logs are deleted first |
| `fileNamePattern`     | Includes the date in the log file name to enable daily file rotation             |
| `cleanHistoryOnStart` | Deletes old logs immediately when the application starts                         |
| Compression Settings  | Logs are usually automatically compressed, typically as `.gz` files              |
### Example (Logback XML Configuration)
```xml
<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
    <fileNamePattern>logs/myapp.%d{yyyy-MM-dd}.log</fileNamePattern>
    <maxHistory>90</maxHistory>
    <totalSizeCap>10GB</totalSizeCap>
    <cleanHistoryOnStart>true</cleanHistoryOnStart>
</rollingPolicy>
```

- This configuration rotates log files daily, retains them for 90 days, and removes the oldest logs when the total size exceeds 10GB.
- `%d{yyyy-MM-dd}`
	- date pattern inside `fileNamePattern` **controls the frequency of rotation**
	- so for us it means the log file name includes the date formatted like this, which causes Logback to create a new log file every day
	- If it's something like `%d{yyyy-MM}` (year and month only), it would rotate monthly instead
# Rolling Policy
>[!Definition]
>A **rolling policy** is a *log management strategy* that automatically splits (archives) log files when they become too large or too old, and deletes or compresses older logs.

- In a production environment, it is essential to store logs in files and manage them properly.  
	- Logback provides **rolling policies** based on time, size, or a combination of both.
- Why is a Rolling Policy Needed?
	- If logs keep accumulating in a single file, the size can grow to several GBs or even tens of GBs.
	- This can deplete disk space or slow down log viewing.
	- In production, it is common practice to *retain old logs only for a set period* and *delete them automatically*.
- Also in `logback-spring.xml`
## Main Types of Rolling Policies
- In `logback-spring.xml` 

| Policy Type        | Description                                                                      |
| ------------------ | -------------------------------------------------------------------------------- |
| Time-based Rolling | Creates a new log file at set time intervals (e.g., daily, weekly, monthly)      |
| Size-based Rolling | Creates a new log file when the current one exceeds a certain size (e.g., 10 MB) |
| Time + Size-based  | Splits log files when both time and size thresholds are met                      |
#### Time-based rolling
```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>logs/myapp.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
		    <!-- 매일 파일 생성 -->
		    <fileNamePattern>logs/myapp.%d{yyyy-MM-dd}.log</fileNamePattern>
		    <!-- 최대 30일치 보관 -->
		    <maxHistory>30</maxHistory>
		</rollingPolicy>
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```
- **`fileNamePattern`**: Creates a new log file daily, e.g., `logs/myapp.2025-07-23.log`, `logs/myapp.2025-07-24.log`.
- **`maxHistory`**: Automatically deletes logs older than 30 days.
#### Time + Size-based
```xml
<appender name="SIZE_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>logs/myapp.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
		    <fileNamePattern>logs/myapp.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
		    <maxFileSize>10MB</maxFileSize>
		    <maxHistory>14</maxHistory>
		</rollingPolicy>
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```
- Created daily, and if a single log file exceeds 10 MB, it is split into files like `...1.log`, `...2.log`.
- Retains logs for a maximum of 14 days.


# Changing Log levels
Sometimes you may want to dynamically adjust log levels while the application is running. Spring Boot Actuator provides a `loggers` endpoint that allows you to change log levels at runtime.

1. Add Actuator dependency in `build.gradle`
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```
- Without this, actuator endpoints like `/actuator/loggers` won’t exist.

2. `application.properties` 설정
```yml
management.endpoints.web.exposure.include=loggers
management.endpoint.loggers.enabled=true

management:
	endpoints:
		web:
			exposure:
				include: loggers
		loggers:
			enabled: true
```
- **Configure Actuator in `application.properties` or `application.yml`**  
- You have to enable the `loggers` endpoint (usually included in `management.endpoints.web.exposure.include`) and possibly secure it

3. Send the cURL (or HTTP) request to change the log level at runtime
```bash
curl -X POST -H "Content-Type: application/json" \
     -d '{"configuredLevel": "DEBUG"}' \
     http://localhost:8080/actuator/loggers/com.example.myapp
```
- This is the actual action that updates the logging level dynamically.
- This request changes the log level of the `com.example.myapp` package to DEBUG. You can also set it to TRACE, INFO, WARN, ERROR, or other valid levels as needed.
# `<springProfile>`
- The `<springProfile>` tag in `logback-spring.xml` allows you to apply different logging configurations depending on the active Spring profile (e.g., `dev`, `prod`).
- 

Basic example
```xml
<configuration>

  <!-- Common appender for all profiles -->
  <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <!-- Development profile settings -->
  <springProfile name="dev">
    <root level="DEBUG">
      <appender-ref ref="CONSOLE" />
    </root>
  </springProfile>

 ....

</configuration>

```

# Tips
- When using `logback-spring.xml`, make sure to utilize the `<springProfile>` tag supported by Spring Boot to separate configurations for each environment.
- Create a `/logs` folder and manage log files outside the application directory for easier integration with monitoring and log collection tools in production.
- Production
	- Set the log level to `INFO` or `WARN`; in development, use `DEBUG`.
	- Always configure file output and rolling policies to prevent disk space exhaustion.
- Generally
	- Never include sensitive information in log messages (e.g., passwords, social security numbers).
	- Set log levels minimally for features that have a significant impact on service stability.
	- Console output is usually sufficient in development, but file-based logging is recommended in test and production environments.
	- To integrate logs with external systems (e.g., ELK, Grafana, CloudWatch), a file-based logging strategy is essential as the foundation.
