# Spring Background
- [[EJB briefly explained]]
- [[Library VS Framework]]
# Spring Framework
> Spring = Spring Framework
- [[WAS vs Web Server]]
- [[POJO vs JavaBeans VS Spring Beans]]
> Main philosophy
- [[Spring Framework & Ecosystem]]
- [[Spring's main philosophies]]
	- [[Spring's main philosophies#POJO Based Development|POJO Based Development]]
	- [[Spring's main philosophies#Object-Oriented Design Principles|Object-Oriented Design Principles]]
	- [[Spring's main philosophies#Test-Driven Development (TDD)|Test-Driven Development (TDD)]]
- [[Reading Spring official docs]]
# Core Concepts
- [[Bean]]
- [[IoC (Inversion of Control)]]
- [[DI (Dependency Injection)]]
- [[AOP (Aspect Oriented Programming)]]
# Spring Boot Project
- [[Understanding Spring Boot - Auto-configuration, Standalone applications, Embedded Servers]]
	- Auto-configuration
	- Standalone applications
	- Embedded Servers
- [[Basic Dependencies]]
- [[Spring's Core Modules]]
	- Understanding Dependency Structure
	- `spring-core`, `spring-beans`, `spring-context`, `spring-web`, `spring-aop`
- [[Starting a Spring Boot Application]]
	- main method & [[@SpringBootApplication]]
- [[Application Execution Process]]
- [[@ConfigurationProperties]]
- [[Various Dependencies]]
#### Project Start 
- [[Spring Boot Project Initialization]]
- [[SpringBoot Project Structure]]
	- `src/main/java`
		- [[@SpringBootApplication]], `@Component`, `@Bean`
		- DTO/Mapper
	- `src/main/resources`
	- `src/test`
	- Convention over Configuration
- [[Gradle]]
- [[Spring Boot Starter]]
# Patterns/Architecture
>Spring Boot Architecture Pattern 
- [[⭐Layered Architecture]]
- [[⭐Spring MVC]] (part of layered architecture)
- [[Event Architecture]]
- [[Data Transfer Object (DTO)]]
- [[Mapper and MapStruct]]
- [[Spring Servlet]] (Tomcat and `DispatcherServlet` Overview)
# Spring MVC
- [[Reactive Programming (&Blocking, Non-Blocking IO)]]
- [[⭐Spring MVC]] (part of layered architecture)
	- [[Getting Started with Spring MVC]]
	- [[⭐ Handler Methods]]
	- [[⭐Handler Method Parameters]] (in Controller Handler Methods)
	- [[Handling File Uploads]]
	- [[Handling Response Data]]
- Web
	- [[HTTP Fundamentals]]
	- [[HTTP requests in SpringBoot]]
- [[APIs]]
	- [[API in SpringBoot]]
	- [[API documentation (Spring REST DOCS, Swagger UI)]]
- [[Microservice Architecture (MSA)]]
- [[Serverless Architectures & APIs]]
# Data Access & Persistence + DB
- [[Spring Data Access Technologies]] - SQL centric and ORM
	- [[JPA (Jakarta Persistence API)]]
	- [[Spring Data JPA]]
- [[Entity Design]] - single entity
- [[Entity Relationship Mapping]] - relationship between entities
- [[Query Auto Generation]]
- [[QueryDSL]]
- [[The big picture flow (Spring + PostgreSQL)]]
- [[ddl-auto options in yaml|data_access/ddl-auto options in yaml]]
## Paginations
- [[Offset vs Cursor pagination comparison]]
	- [[Offset-based Pagination (Spring)]]
	- [[Cursor-based pagination (Spring)]]
## Transactions
- [[Transaction]]
- [[Transaction - ACID]]
- [[Transaction Isolation and Concurrency Issues]]
- Transactions in Spring
	- [[Declarative vs. Programmatic Transaction]]
	- [[Spring AOP and Transactions]]
	- [[@Transactional and the Self-Invocation Problem]] (Why internal calls fail)
	- [[Transaction Propagation]]
# Improving Spring Application Stability
>Potential problems
- [[4 Potential problems in a Spring application]]
>Strategies
- [[Exception handling]]
	- [[Exception handling Structure & Hierarchy]]
	- [[Systematizing Error Codes]]
	- [[System exception VS Business Exception]]
	- [[Exception Locations in a Spring Application]]
- [[Logging]]
	- [[Logging in Spring - Logback]] (and SLF4J)
	- [[Writing Logging Messages]]
	- [[Log Search & Analyzing]]
- [[Bean validation (Input validation)]]
	- [[Input validation locations (service, controller)]] - `@Valid, @Validated`
	- [[Custom Validator]]
	- [[Validating Composite Objects]] - `@Valid`
- [[Monitoring]]
	- [[API performance metrics]]
	- [[Spring Boot Actuator]]
# Spring TDD
- [[Test-Driven Development (TDD)]]
- [[Testing in Spring]]
	- [[Unit Test]] 
	- [[Functional Test]] 
	- [[Integration Test]] 
	- [[Slice Test]]  (unfinished)
	- [End-to-End Test](https://calm-individual-12a.notion.site/End-to-End-247c6b70982881718da4cf5c95d92b3d)
- Layers
	- [[API (Controller) Layer Testing]]
	- [[Service Layer Testing]]
	- [[Data Access (Repository) Layer Testing]]
- [[Spring test module - spring-boot-starter-test]]
	- [[JUnit 5]]
- Configs & Settings
	- [[Configuration yaml settings for logging + testing]]
	- [[Initializing test data with @Sql]]
- [[Mockito]]
- [[Writing Test Code]]

>Sprint 7
- [[Debugging a Silent Spring @ExceptionHandler Failure]]
- [[ControllerTest - GSON VS ObjectMapper]]
# Other Spring Features
- [[Application Events and Event Listeners]]
- [[Spring Batch Processing]]
- [[Spring Boot + MongoDB]]
- [[Spring Boot + AWS SDK (S3)]]
- [[Docker, Spring Boot, MongoDB, and yaml files]]
- [[Using Websockets in SpringBoot]]
- [[Using Redis in SpringBoot]]
# Spring Security
- [[Spring Security]]
- [[Notes Spring Security 1]]