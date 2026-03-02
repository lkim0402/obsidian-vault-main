- Logging differs between development and production environments
	- Development - devs need detailed logs for debugging
	- Production - keep logs minimal to preserve system performance, security, and disk space
- Use `@Profile` or `spring.profiles.active`
- To separate configurations in [[Logging in Spring - Logback|Logback]] by using `<springProfile>` in the logging configuration file

- logging
	- https://calm-individual-12a.notion.site/246c6b70982881ebb9a9fb7d17db6e81
- db 설정
	- https://calm-individual-12a.notion.site/231c6b70982881cbb61acae0fbdab56b

# Example structure (`yml` files)
- In [[🐣Spring & SpringBoot|Spring Boot]], you can create test-specific configuration files under `src/test/resources` to separate the test environment completely from production. This allows safe and predictable testing.
```
src
├── main
│   └── resources
│       ├── application.yml
│       ├── application-dev.yml
│       ├── application-test.yml
│       ├── application-prod.yml
│       └── logback-spring.yml
```

# Active profiles for testing
- Spring allows you to create `application-{profile}.yml` files and activate specific profiles during testing to apply corresponding settings.
- Here are the different ways to activate Spring profiles for testing (pick one)
- [[Configuration yaml settings for logging + testing#`application-test.yml`|Dev and Prod yaml settings for logging + testing: application-test.yml]]
	- Before choosing to do any of these, you should create `application-test.yml` in `src/test/resources` (look above)
#### `@ActiveProfiles`
```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceTest {
    // test code
}
```
- Applying `@ActiveProfiles("test")` on a test class activates the `application-test.yml` profile only for that test.

#### `src/test/resources/application.yml`
- You can set the default profile globally here
```yml
spring:
  profiles:
    active: test
```
- This sets the `test` profile as the default for all tests

#### `build.gradle` - specifying system property
```groovy
// build.gradle
test {
  systemProperty 'spring.profiles.active', 'test'
}
```
- In [[Gradle]], this passes `spring.profiles.active=test` as a system property when running tests.

#### Using `@TestPropertySource`
```java
@SpringBootTest
@TestPropertySource(
    properties = "spring.profiles.active=test",
    locations = "classpath:custom-test.properties"
)
class CustomPropertyTest {
    // test code
}
```
- `@TestPropertySource` lets you specify a custom properties file or inline properties for a test.
- In practice, `@ActiveProfiles` and setting the default profile in `src/test/resources/application.yml` are the most common ways to configure test profiles.

# Directory & yaml Files Example (log/test)
```
src
├── main
│   └── resources
│       ├── application.yml
│       ├── application-dev.yml
│       ├── application-test.yml
│       ├── application-prod.yml
│       └── logback-spring.yml
```
- `application.yml` - Common settings and profile activation
- `application-dev.yml` - Development environment settings only
- `application-test.yml` - Test environment settings only
- `application-prod.yml` - Production environment settings only
- `logback-spring.yml` - Common logging settings and profile-based logging configuration

#### `application.yml`
```yml
spring:
  profiles:
    active: dev  # 실행 시 기본 활성화 프로필 (dev)

  # db settings for production 
  datasource:
    # Connect to a real MySQL database
    url: jdbc:mysql://prod-db-server:3306/my_app
    username: prod_user
    password: ${PROD_DB_PASSWORD} # Loaded from environment variables
  jpa:
    hibernate:
      ddl-auto: validate # Don't automatically change the real database
```
#### `application-dev.yml`
- Spring Boot uses the naming convention of `application-{profile}.yml` (or `.properties`) to load profile-specific configuration files automatically
```yml
logging:
  level:
    root: DEBUG
    com.example.myapp: DEBUG
    org.hibernate: WARN
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"
```
#### `application-test.yml`
```yml
######### logging settings #########
logging:
  level:
    root: WARN
    com.example.myapp: DEBUG
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"

######### db settings #########
spring:
  # db settings for test
  datasource:
    # Use a fast, temporary in-memory database for each test run
    url: jdbc:h2:mem:testdb
    username: sa
    password: password
  jpa:
    hibernate:
      ddl-auto: create-drop # Create the schema at the start, drop it at the end
  show-sql: true            # Show SQL logs for debugging
main:
	allow-bean-definition-overriding: true  # Allow overriding beans in tests
```
- uses an H2 in-memory [[🗃️Database|database]] for testing 
- sets Hibernate SQL logging to `DEBUG` level
-  `ddl-auto: create-drop` option => ensures tables are *created at test start and dropped afterward*

#### `application-prod.yml`
```yml
logging:
  level:
    root: WARN
    com.example.myapp: INFO
  file:
    name: logs/myapp.log
```
#### `logback-spring.yml`
- [[Logging in Spring - Logback#Rolling Policy|Logging in Spring - Logback: Rolling Policy]]
- `yaml` files are for general [[🐣Spring & SpringBoot|Spring Boot]] properties and basic logging levels, but **not for complex Logback setup** such as rolling policies.
	- offers a limited YAML-based wrapper configuration for some logging properties, including rolling policies, but it is not a full replacement for the XML config
	- NOT a full alternative to XML config
```yml
# 공통 및 운영환경용 롤링 설정 포함
logging:
  file:
    name: logs/myapp.log

  logback:
    rollingpolicy:
      file-name-pattern: logs/myapp.%d{yyyy-MM-dd}.log
      max-history: 90                 # 보관 기간: 90일
      total-size-cap: 10GB            # 최대 저장 용량: 10GB
      clean-history-on-start: true    # 애플리케이션 시작 시 오래된 로그 삭제
```
- `logback-spring.yml` replaces XML-based configuration and is automatically parsed by Spring Boot to apply logging settings.
- `logging.logback.rollingpolicy` is only supported in `logback-spring.yml`, so do not declare it in `application-*.yml`.
- Manage profile-specific logging levels (e.g., development, production) in `application-*.yml`, and configure actual log file policies in `logback-spring.yml`.

---
# Conventions and good practices 
#### Production: `application-prod.yml`
- You want to see only what's necessary to monitor the health of the application and react to critical problems. 
- High-volume logging can impact performance and fill up disk space quickly
- Good practices
	- **Root Log Level: `INFO` or `WARN`**: Set the default logging level to `INFO`. 
		- This logs application startup messages, important events, and any custom `INFO` logs you've written. 
		- Use `WARN` if you want to be even more conservative and only log potential issues.
	- **File Logging**: **Always log to a file** in production. 
		- The console is unreliable for long-term storage and analysis. You should configure the log file location and rotation policy to prevent it from growing too large.
	- **Specific Packages: `ERROR` or `WARN`**: Suppress verbose logs from external libraries. 
		- For example, database-related libraries like Hibernate or JDBC are often very chatty. In production, you generally only need to see their `ERROR` or `WARN` logs.
	- **Structured Logging**: Consider using a structured format like JSON for easier ingestion and analysis by log aggregation tools (e.g., Elasticsearch, Splunk).
Example:
```yaml
logging:
  level:
    root: INFO # Default level for all loggers
    org.springframework: WARN # Suppress most Spring framework logs
    org.hibernate.SQL: WARN # Suppress detailed Hibernate SQL logs
    org.hibernate.type.descriptor.sql.BasicBinder: WARN # Suppress parameter binding info
    com.yourcompany: INFO # Set your application's package to INFO
  file:
    name: /var/log/your-app/your-app.log # Log to a file
  logback:
    rollingpolicy:
      max-file-size: 10MB # Rotate log file at 10MB
      max-history: 10 # Keep up to 10 old log files
```
#### Test: `application-test.yml`
- You want to see enough detail to *diagnose test failures without being overwhelmed*. This is similar to development but often more focused
- **Good Practices:**
	- **Root Log Level: `INFO` or `WARN`**: Start with a moderate root level.
	- **SQL Logging: `DEBUG` or `TRACE`**: This is where you need to be verbose. To debug a failing integration test, it's invaluable to see the exact SQL queries being executed and their parameter values.
	- **Application-specific: `DEBUG`**: Set your application's package to `DEBUG` to see detailed logs about your business logic.
	- **Console Only**: It's usually sufficient to log to the console (`stdout`) during testing. File logging can be overkill and just creates more files to manage.
```yaml
logging:
  level:
    root: WARN # Suppress noise from non-essential libraries
    com.yourcompany: DEBUG # Verbose logging for your code
    org.hibernate.SQL: DEBUG # See all SQL statements
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE # See SQL parameter values
```
#### Dev:  `application-dev.yml`
- You want to see everything to debug issues quickly - *most verbose configuration*
- **Good Practices:**
	- **Root Log Level: `INFO`**: A good starting point. You can go lower if you want to see even more framework logs.
	- **Application-specific: `DEBUG`**: Your own code should be the priority. Set your base package to `DEBUG`.
	- **SQL Logging: `DEBUG`**: Like in testing, seeing the SQL queries is extremely helpful during development.
	- **Console Output**: Console logging is perfect for development. It's fast, and you can see real-time logs in your IDE.
	- **`spring.jpa.show-sql`**: Use this Spring Boot property (or the Hibernate equivalent `format_sql`) to format and display SQL queries.
```yaml
# A common pattern is to have a base application.yml with shared settings,
# and this profile-specific file overrides only the logging.
# Assume that show-sql is in application.yml.
# spring.jpa.show-sql: true

logging:
  level:
    root: INFO
    com.yourcompany: DEBUG # Your application's package
    org.springframework.web: INFO # See Spring MVC mappings and other high-level info
    org.hibernate.SQL: DEBUG # All executed SQL
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE # Parameter values
```