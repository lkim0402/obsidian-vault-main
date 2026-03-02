
>[!Spring Framework]
>The Spring Framework is an open-source application framework for the Java platform, often simply called **Spring**

-  [[🐣Spring & SpringBoot]]
- Spring is an umbrella term for a massive ecosystem
- *Application framework*
	- A set of common software functionalities that provides a foundational structure for developing an application; eases the effort of writing an application by taking out the effort of writing all the program code from scratch 
	- No need to use everything; Depending on the requirements of the app you make, choose the right parts to use
	- The Spring framework (shortly, Spring) is an application framework that is part of the java ecosystem
- Building applications is like an iceberg
	- ![[spring-boot-iceberg.png]]
- Follows the [[Spring's main philosophies]]
# Spring Ecosystem
- Diagram - like a ⭐solar system ⭐
	![[spring-solar-system.png]]
	- Spring Framework has Spring Core in the middle, which holds spring mvc, spring data access, and spring testing together
- Spring offers an entire ecosystem formed of many **projects** used in application development. 
- **Project**
	- A distinct, standalone software library (or collection of libraries) developed by the Spring team to solve a specific problem domain
	- Each project is dedicated to a specific domain, and when implementing an app, you might use more of these projects to implement the functionality you desire
	- Maintained separately, have their own version numbers, and their own documentation
## ⭐ Spring Framework (core, web, testing, data access)
- This is like the "Mother" Project that is the starting point for all other projects
### Spring Core
- One of the most fundamental parts of Spring that includes *foundational capabilities*
	- Every other project is built on top of this)
- One of these features is the Spring Context
	- a fundamental capability of the Spring framework that enables Spring to manage instances of your app
	- Provides the foundational mechanisms to integrate into apps, like [[IoC (Inversion of Control)]]
- The Spring aspects functionality
	- [[AOP (Aspect Oriented Programming)]]
	- help Spring intercept and manipulate methods you define in your app
- The Spring Expression Language (SpEL)
	- enables you to describe configurations for Spring using a specific language
### Spring Web
>[!Spring web]
>This part of the framework is responsible for handling web requests. It acts as the entry point to your application, receiving requests, passing them to your business logic (services), and returning responses. Spring provides two different frameworks for this.

-  [[⭐Spring MVC|Spring MVC]]
	- The original, widely-used web framework for building synchronous, blocking web applications that serve [[HTTP Fundamentals#HTTP Request Methods|HTTP requests]].
	- It is the default choice in `spring-boot-starter-web`.
	- **Model**: Uses the traditional **"Thread-per-Request" model**.
	- **Best for**: Standard CRUD applications, REST APIs, and server-side rendered websites where a simple, blocking workflow is sufficient. It has a mature ecosystem with extensive documentation and community support.
- Spring WebFlux
	-  A modern, non-blocking, reactive web framework designed for asynchronous applications that can handle massive concurrency with a small number of threads.
	- **Model**: Uses an **Event Loop** concurrency model.
	- **Best for**: High-performance microservices, streaming applications (like video or live data feeds), and services that need to call other APIs without wasting threads on waiting.)
- [[Reactive Programming (&Blocking, Non-Blocking IO)]]
### Spring Testing
- [[Test-Driven Development (TDD)]]
- [[Testing in Spring]]
### Spring Data Access
- [[Spring Data JPA]]
- [[Spring Data Access Technologies]]
## Spring Boot
- A project part of the Spring ecosystem that introduces the concept of “convention over configuration.”
- **Default configuration**
	- Instead of setting up all the configurations of a framework yourself, Spring Boot offers you a default configuration that you can customize as needed
- **Embedded Server**
	- _Traditional Spring:_ You build a `.war` file and deploy it to an external Tomcat server.
    - _Spring Boot:_ The server (Tomcat, Jetty, or Netty) is embedded _inside_ the `.jar` file. The app runs itself (`java -jar app.jar`).

## Spring Data
>[!Spring data]
>A project that provides a consistent programming model and an abstracted approach for various data stores (RDBMS, NoSQL, search engines, etc.)
>- Significantly enhances the productivity and consistency of data access logic by **automating complex DAO (Repository) implementations**

- Basically enables you to easily connect to databases and use the persistence layer with a minimum number of lines of code written
	- [[SQL (Relational) vs NoSQL (Non Relational)]]
	- Spring Data Access
		- Module of spring code
		- contains fundamental data access implementations like transaction mechanism and JDBC tools
	- Spring Data
		- enhances those databases and makes development much more accessible
- Collection of sub-projects
	- **Spring Data JPA**: JPA-based ORM abstraction.
		- [[Spring Data JPA]]
	- **Spring Data [[🍃MongoDB]], Redis, Cassandra, etc.**: Support for NoSQL databases.
	- **Spring Data Elasticsearch**: Integration with search engines.
	- **Spring Data REST**: Automatically exposes Repositories as [[REST API|REST APIs]].
- Key features
	- **Automatic CRUD Code Generation**: Automatically implements query methods based on method names. 
	- **Standardization of Common Interfaces**: Development based on extending common interfaces like `CrudRepository` and `JpaRepository`.
	- **Data Store Independence**: Allows *changing data stores with minimal code modification*.
	- **Built-in Various Auxiliary Features**: Includes pagination, sorting, dynamic queries, and Auditing.
## Spring Security
>[!Spring Security]
>A security framework responsible for **Authentication** and **Authorization** in Spring-based applications
- Key features
	- **Provides a Separate Structure for Authentication/Authorization:**
	    - **Authentication**: Handles login, session management, and tokens.
	    - **Authorization**: Manages permission checks and access control.
- **Supports various authentication methods**: Includes session-based, token-based (JWT), OAuth2, and more.
- **Filter Chain-based Security Processing**: Applies centralized security to all requests via Servlet Filters.
- **Declarative Method Security Features**: Utilizes annotations like `@Secured`, `@PreAuthorize`, `@PostAuthorize`.
- **Security Vulnerability Countermeasures**: Includes built-in defense features against web vulnerabilities such as CSRF, XSS, Session Fixation, and Clickjacking.
## Spring Cloud
>[!Spring Cloud]
>Provides integrated support for infrastructure components specifically designed for **cloud-native applications**, particularly those built with a **Microservices Architecture (MSA)**. 

- Helps devs *easily configure frequently required patterns in complex distributed systems*, such as configuration servers, service discovery, [[Elastic Load Balancer (ELB)|load balancing]], and [[API Gateway|API gateways]].
- Key features
	- **Config Server**: Centralized management of service configuration files from external sources like Git.
	- **Service Discovery (Eureka)**: Enables services to dynamically register, find, and connect with each other.
	- **API Gateway (Spring Cloud Gateway)**: Routes and filters external requests, handling common processes like authentication, logging, and rate limiting.
	- **Logging/Monitoring**: Supports distributed tracing with Sleuth + Zipkin.
	- **Resilience Pattern Support**: Provides features like Circuit Breaker (Resilience4j, Hystrix), Retry, and Rate Limiting.
- Practical Application Points
	- Enables easy configuration of automation, scalability, and fault tolerance in production environments.
	- Standardizes connection, security, and monitoring among Spring Boot-based microservices.
	- Can be flexibly applied across various cloud platforms such as [[AWS services]], GCP, and Azure.

# When would you use spring?
- Backend app
	- [[Backend Engineering]]
	- Diagram - the possibilities of using spring in backend are endless!
		![[spring-capabilities.png]]
	- Spring basically gives all the tools you need (integration with other apps, database technologies, testing, etc)
- An automation testing framework
	- *Automation testing* - implementing a software that development teams use to make sure an application behaves as expected
	- It's good to automate the test cases! (for more complex systems, manually testing all the flows isn't even an option)
		- You can have a separate team that implements an app that only validates the flows of the tested system & schedule it regularly to get feedback as soon as possible
	- For end-to-end testing
	- A testing app might need to connect to databases / communicate with other systems. The devs can use components of the Spring ecosystem to simplify the process
- A desktop app
- A mobile app
	- Spring for Android project
# When NOT to use frameworks (like Spring)
- Sometimes using a tool that's too much for the job might consume more energy and also obtain a worse result
- Some potential reasons
	- You need to implement a particular functionality with a footprint as small as possible (footprint = storage memory occupied by the app's files)
	- Specific security requirements force u to implement only custom code w/o use of open source
	- You need lots of customization
	- You alr have a functional app, by changing it to use a framework won't benefit you
# Connecting Projects
>[!tip]
>- Each Spring project can be used independently, yet they are designed to be organically connected, all leveraging the Spring Framework as their common foundation. 
>  - This allows for flexible system design by combining them freely to meet diverse architectural and requirement needs.

- **Spring Boot + Spring Data**: Minimizes configuration + Automates Repository implementation → Rapid development of data-centric APIs.
- **Spring Boot + Spring Security**: Quickly configures authentication/authorization logic, and builds OAuth2 or JWT-based authentication systems.
- **Spring Boot + Spring Cloud**: Configures infrastructure patterns (Config Server, Gateway, Discovery, etc.) essential for microservices environments.
- **Spring Boot + Spring AOP**: Separates common logic (logging, transactions, etc.) from core logic to achieve a clean architecture.
- **Spring MVC + Spring Security + Spring Data JPA**: Forms the standard structure for traditional CRUD-based web/REST services.