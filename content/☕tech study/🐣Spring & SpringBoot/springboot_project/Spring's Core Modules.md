# The Dependency Structure
#### Spring Framework Structure
- Not one library, but a **modular platform** of many hierarchical components
- Each module serves a distinct purpose (e.g., IoC, AOP, Web, Data)
- Knowing the module structure clarifies **how Spring works** and where specific features reside
#### Checking the Gradle Dependency Tree
- Using features in Spring Boot pulls in **multiple Spring modules** via a **dependency tree**
- To view all dependencies across configurations (compile, runtime, test):
```bash
./gradlew dependencies
# 특정 Configuration에서 compileClasspath라는 이름의 의존성만 출력
./gradlew dependencies --configuration compileClasspath
```
#### Hierarchical Position of Key Modules
![[module-hierarchy.png|600]]
- `spring-core`
	- **Most fundamental Spring module** (Foundation for nearly all other Spring modules)
	- Provides utilities: **reflection, type conversion, collections, exception handling**
	- Reminds me of `Java`'s `Object`
	- Key classes: `ObjectUtils`, `Assert`, `ClassUtils`, `PropertyEditor`
- `spring-beans`
	- Core module for [[DI (Dependency Injection)]]
	- Handles **[[Bean]] creation, registration, and injection**
	- Powers annotations like `@Component`, `@Bean`, and use of `ApplicationContext`
	- Built on top of `spring-core`.
-  `spring-context`
	- **Provides full container environment**:
	    - [[Bean]] registration, event publishing, message resolution
	- Core of **`ApplicationContext`** implementations
	- Enables: `@Configuration`, `@Autowired`, `@Value`, `ApplicationContextAware`
	- **Backbone for runtime environment** in Spring Boot & MVC
	- Depends on: `spring-core`, `spring-beans`
- `spring-web`
	- The foundational layer for web functionalities, including HTTP request processing, Servlet API, `RestTemplate`, `WebClient`, etc.
		-  `spring-web` supports both _API server roles_ (receiving) and _API client roles_ (sending), making it essential for building distributed systems in Spring.
	- Contains core web functionalities that precede the MVC stage.
		- servlet
	- Depends on `spring-beans`, `spring-core`.
-  `spring-aop`
	- Provides [[AOP (Aspect Oriented Programming)|AOP]]-related functionalities (`@Aspect`, `@Before`, `@After`, Proxy-based Interceptors, etc.).
	- Proxy based AOP
	- Essential for implementing Spring's own transaction and security logic.
- `SpEL`
	- Spring사용에 필요한 문법이 담김

Others
- `spring-tx`, `spring-jdbc`
	- **데이터 액세스와 트랜잭션 처리 기능 분리**
	- `spring-jdbc`: JDBC 연동, `JdbcTemplate`, SQL 예외 처리
		- allows to interact with database
	- `spring-tx`: 선언적/프로그래밍 방식 트랜잭션 처리, `@Transactional` 지원
	- ORM 연동(`Hibernate`, `JPA`) 시에도 `spring-tx`가 중간 조정자 역할
		- allows us to interact with a db
```
Java JDBC API
	↑
Spring JDBC
	↑
Spring Data JPA
```

| Module           | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| spring-messaging | WebSocket, STOMP 등 메시징 추상화<br>- 실시간 통신                           |
| spring-aspects   | AspectJ와 통합할 때 사용하는 보조 모듈                                        |
| spring-test      | 테스트 지원 (@SpringBootTest, @WebMvcTest 등)<br>- Mockito 같은 것도 활용을 함 |

Spring modules have unidirectional dependencies
- functionalities at a higher level implicitly include more lower-level modules
- **The Arrows (`↑`) mean "depends on" or "is built on top of"**
```
spring-core
   ↑
spring-beans
   ↑
spring-context
   ↑
spring-web
   ↑
spring-webmvc
```
- For instance, while `@RestController` is defined within `spring-webmvc`, its functionality requires the combined operation of features from `spring-context`, `spring-beans`, and `spring-core`.
- There are many many more modules, but these modules form the absolutely essential core and the most common web stack


#### Core Module Hierarchy
```
spring-core
   ↑
spring-beans
   ↑
spring-context
```

#### Web Module Hierarchy
```
spring-core
   ↑
spring-web
   ↑
spring-webmvc
```

- Like a toolbox, you can pick what you only want to use
- Spring is like a big toolbox that contains other toolboxes (instead of having all the tools together)

>[!Summary]
>- Spring Framework는 단일 라이브러리가 아니라, **기능별 책임을 분리한 다수의 모듈로 구성된 계층형 프레임워크**다.
>  - Gradle의 의존성 트리를 통해 `spring-core`부터 `spring-webmvc`까지의 **계층 구조와 의존 흐름을 시각적으로 이해**할 수 있으며, 이는 실무에서 문제를 디버깅하거나 설정을 이해하는 데 매우 중요한 기반이 된다.
>  - Spring의 기반은 `spring-core`로부터 시작되며, `spring-beans`와 `spring-context`를 거쳐 **ApplicationContext 기반의 런타임 환경이 완성**된다.
>  - 웹 계층에서는 `spring-web`이 HTTP 기반 기능을 담당하고, `spring-webmvc`가 MVC 구조를 완성하여 **웹 애플리케이션 프레임워크로서의 Spring**을 형성한다.
>  - `spring-aop`, `spring-jdbc`, `spring-tx` 등의 모듈은 횡단 관심사나 데이터 접근 등의 기능을 **선택적으로 확장 가능**하게 제공하며, 필요에 따라 Spring 애플리케이션에 유연하게 통합된다.

