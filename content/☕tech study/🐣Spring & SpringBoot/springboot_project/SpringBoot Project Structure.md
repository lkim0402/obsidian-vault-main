- The convention & structure will not change
- Follow common structure for easy collaboration
	- Use **lowercase only**, no underscores or starting with numbers
# `src/main/java`
> the core directory for your executable application 
- Where the business logic is
- main functionalities defined here
- package structure is automatically generated based on your **`GroupId`** and **`ArtifactId`**

계층형 분리 구조 - Layered Separation
```
src/
└── main/
    └── java/
        └── com/sprint/practice/
            // 메인 실행 클래스
            ├── PracticeApplication.java
            ├── controller/
            ├── service/
            ├── repository/
            └── domain/ or entity/
```

도메인별 구조 분리 - Domain-Oriented Structure
```
src/
└── main/
    └── java/
        └── com/sprint/practice/
            // 메인 실행 클래스
            ├── PracticeApplication.java    
            └── Product/
		            ├── controller/
		            ├── service/
		            ├── repository/
		            └── domain/ or entity/
			└── Service/
				├── controller/
				....
```
- When you have a small project (big projects have 1000s of domains so its unrealistic to do it like this)

|Package|Role|Examples|
|---|---|---|
|`com.example.demo`|Root package (main app)|`PracticeApplication.java`|
|`controller`|Handles HTTP requests|`MemberController`|
|`service`|Business logic|`OrderService`, `AuthService`|
|`repository`|Data access (CRUD)|`MemberRepository`|
|`domain`/`entity`|Core entities & domain models|`Member`, `Order`, `Product`|
#### `@SpringBootApplication`
- [[@SpringBootApplication]]
#### `@Component`
- 얘가 달려있으면 그 클래스는 [[Bean]]이다
#### `@Bean`
- 🔧 The **return value** of the method annotated with `@Bean` is **registered as a Spring bean** in the **ApplicationContext**.
- use the method name as its default ID 
```java
@Configuration // Indicates this class provides Spring Bean definitions
public class AppConfig {

    @Bean // This method will create and configure a Spring Bean
    public MyService myCustomService() {
        return new MyService(); // The returned MyService object is registered as a bean
    }

```

#### DTO/Mapper
> In [[⭐Layered Architecture]], we can use DTO to transfer data between the layers <<
- DTO (Data Transfer Object)
	- Purpose: Transfer data safely between app layers
		- You can use from client to controller
	- Immutability: Usually *immutable* to keep data consistent (guarantees that data won't change)
	- Safety: Controls which fields are exposed to avoid leaking internal data
- Mapper
	- Converts between DTOs and Entities
	- Example
		- Client sends `MemberDto` → Mapper converts to Member entity for service
		- Service returns `Member` entity → Mapper converts to `MemberDto` for client

# `src/main/resources`
> Stores non-Java resource files
- where you configure your application's runtime environment and provide static assets for client responses

|Path|Purpose|
|---|---|
|`application.yaml` / `.properties`|Core app config (DB, port, logging)|
|`static/`|Static files (HTML, CSS, JS, images)|
|`templates/`|Server-side templates (Thymeleaf, etc.)|
|`banner.txt`|Custom startup ASCII banner|
|`messages.properties`|i18n message localization|
|Other resources|SQL scripts, certs, JSON, etc.|
#### `application.yml` / `.properties`
> basically `.env` + all other settings
- `application.properties`
	- Uses `.` to define hierarchy
	- was the default, has higher priority
- `application.yaml`
	- Uses **indentation** to define hierarchy
	- fundamentally same with `.properties`, just different syntax
	- more commonly used
	- There are even online tools that can convert between .properties and .yaml formats
- Managing data + sensitive data
	- Examples
		- AWS account credentials
		- API keys
		- Database connection details
	- you should reference external secrets management services, the actual sensitive value is retrieved at runtime from a secure location
		- `spring.datasource.password={dbpassword}`
		- Github Secrets (used more recently)
		- AWS Parameter Store (can be expensive)
#### `static/`
- Files in `src/main/resources/static/` served **as-is** (no processing)
- Accessible via root URL (e.g., `static/index.html` → `http://localhost:8080/index.html`)
- Used for simple assets: HTML pages, CSS, JS, images, docs, logos
- No controller needed to serve these
#### `templates/`
- template files used for [[CSR vs SSR (+ React server components)#Server-side rendering (SSR)|server-side rendering]] (e.g., with **Thymeleaf**)
- template engine
- `@Controller` vs. `@RestController`
	- @Controller	
		- Server-side rendering (e.g. Thymeleaf)
		- Returns: File path
	- @RestController
		- client side rendering, where frontend framework makes API calls (e.g. for React)
		- Returns:**Raw data** (JSON/XML)

# `src/test`
- Area for **test code** only separate from `src/main`
	- but has the same structure as `src/main`
- Used to **verify** your app works as expected
```
src/
└── main/                                   <- Source code and static resources
    └── java/
    └── resources/
└── test/
    └── java/
        └── com/example/demo/
            └── DemoApplicationTests.java   <- Auto-generated test class
```
- why separate?
	- Keeps test logic apart from app logic
	- Not deployed with the app (the app itself doesn't need tests themselves, it will just take up memory)
	- Build tools ([[Gradle]]/Maven) handle test code separately
- What tests do
	- Verify functionality
	- Prevent regressions
	- Help test complex logic
	- Safety net for refactors/updates
- Test tools
	- Spring Boot conveniently includes the following testing tools by default (as part of `spring-boot-starter-test`):
		- `JUnit5, Mockito, AssertJ, Spring Test`
- During build
	- Tests run first during build App is only packaged (JAR) if all tests pass
	- the final jar file NEVER includes test code in it
### Test Annotations
> Use `@WebMvcTest` for faster, focused tests.

| Annotation        | Purpose                                                                         |
| ----------------- | ------------------------------------------------------------------------------- |
| `@Test`           | Unit test for isolated components                                               |
| `@SpringBootTest` | Full integration test (*heavy - loads full app context*) <br>- 통합테스트 할 때        |
| `@WebMvcTest`     | Web layer slice test (*light - ㅣloads controllers only*)  ****<br>- 슬라이스 테스트 할때 |

# Convention over Configuration
Spring Boot is designed designed to automatically recognize and function if you place files in specific, conventional locations:
- `application.yml` in `src/main/resources` → auto-loaded
- `static/index.html` → served at `/index.html`
- `@Component`/`@Service` in root package → auto-registered
- Test classes in `src/test/java` → auto-detected & run
👉 Less config, faster setup, easier for beginners
#### Considerations
- **Non-standard file locations?** → Manual config needed (`application.yaml`, `build.gradle`)
- **Beginners:** Best to **follow defaults** to avoid issues





