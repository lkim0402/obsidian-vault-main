
>[!Spring boot]
>- A *Wrapper/Launcher* that automatically configures Spring and provides the execution environment.
> - A **platform to quickly start and configure Spring**, and not a separate framework that runs on top of the Spring Framework

- **[[Spring Framework & Ecosystem|Spring (Framework)]]**: Core features like IoC, AOP, MVC.
- **Spring Boot**: Simplifies Spring usage with auto-config, starter deps, embedded servers — minimal setup.
- **Spring Limitations (pre-Boot)**
	- Too much XML config
	- Inconsistent env setup
	- Slow startup (bootstrap)
- Spring boot is also characteristic of [[Microservice Architecture (MSA)|MSA]]
	- Spring Boot + MSA: rapid development, scalability, loose coupling
# Core Value of Spring Boot
Spring Boot is not just **"Spring made easy and simple"**; it's a tool that entirely redefined Spring's complex configuration and execution structure. 
## Auto-configuration
- **Traditional Spring Issues**
	- Manual config required for each feature (e.g., `DataSource`, `DispatcherServlet`, `ViewResolver`)
	- Inconsistent setups across developers, higher risk of config errors
- **Spring Boot's solution**
	- **Auto-configuration**: *Based on the libraries included in the application and its configuration files*, Spring Boot automatically performs appropriate Bean configuration and environment setup.
	- It looks at the **dependencies (libraries) you've included in your project** (e.g., `spring-boot-starter-web`, `spring-boot-starter-data-jpa`).
```mermaid
flowchart TD

    A[Spring Boot Application] -->|depends on| B[spring-boot-starter-web]
    B -->|depends on| C1[spring-boot-starter-tomcat]
    C1 -->|depends on| D1[tomcat-embed-core]
    D1 -->|includes| E1[Tomcat.class]
    
    B -->|depends on| C2[spring-boot-starter]
    C2 -->|depends on| D2[spring-boot-starter-autoconfigure]
    D2 -->|includes| E2[ServletWebServerFactoryConfiguration.class]
    E2 -->|is inner class of| F[EmbeddedTomcat.class]
    
    F -->|@ConditionalOnClass| E1

    %% 스타일링
    classDef box fill:#fdf6e3,stroke:#333,stroke-width:1px;
    class B,C1,C2,D1,D2 box
    class A fill:#d1f3d1,stroke:#333,stroke-width:1px;

```
- This diagram illustrates the process of how an **Embedded Tomcat(내장 톰캣)** is auto-configured in a Spring Boot application. 
	- **Tomcat**: Popular Java servlet container (web  [[Backend & Main Components Explained#Server|server]]) 
		- **Before**: Had to install & configure manually
		- **Now (Spring Boot)** - You don't install Tomcat separately. Spring Boot literally includes a lightweight version of Tomcat _within your single `.jar` file_.
- **`spring-boot-starter-web`**
	- Marks app as a **web app**
	- A starter dependency = a bundle that pulls in required libs a web app needs
		- [[Basic Dependencies]]
	- Includes **embedded Tomcat library (`tomcat-embed-core`)**  **transitively** (meaning, it's a dependency of the starter, so you get it automatically when you add the starter) 
	- Spring Boot **auto-configures** Tomcat based on presence of these libs
- The auto-configuration class, `ServletWebServerFactoryConfiguration`, then activates the Embedded Tomcat based on the `@ConditionalOnClass(Tomcat.class)` condition.
![[springboot-auto-configuration.png|300]]
```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```
The `@SpringBootApplication` annotation internally includes the `@EnableAutoConfiguration` annotation. This allows Spring Boot to perform auto-configuration based on the configuration classes defined in `spring.factories`.
# Standalone applications
#### Traditional problems
- Required three steps to be executed:
	1. Install a WAS (Web Application Server, like Tomcat).
	2. Deploy the application.
	3. Run the WAS.
- A complex process of creating a `.war` file and deploying it to an external server
#### Spring Boot
- Packages entire app into a **single `.jar`**
- Run with: `java -jar myapp.jar`
- Internally includes a embedded **Servlet Container** (e.g., Tomcat, Jetty)  
	- No separate deployment needed — app runs directly (deployment and execution in one step)
#### Core Technology
- The Spring Boot Maven/Gradle plugins create the executable `fat jar`.
- `org.springframework.boot.loader.Launcher` acts as the main class for running the JAR.
####  Practical Benefits
- No separate **WAS install** → deploy the app **directly**
- Simplifies **CI/CD & DevOps**, great for **Docker** workflows
- (Optional) Can be managed by **systemd**, **pm2**, or **Kubernetes** as a process unit
# Embedded Servers
- Traditional Problems (without Embedded Servers)
	- Required deploying **WAR** to external **WAS** (e.g., Tomcat, Jetty)
	- Needed manual install & config
	- **Inconsistent server setups** → environment-specific issues
- Spring Boot's Solution (Embedded Server)
	- Includes **embedded server** by default
	    - **Tomcat** (default), or **Jetty**, **Undertow**
	- Configurable via `application.properties` / `application.yaml`  
	    - No need for external server setup
- Practical Benefits
	- App + server bundled → **minimal environment setup**
	- Config changes (e.g., port, TLS) via **YAML/properties**, no code edits
	- Ideal for **microservices** → easy deployment as self-contained units