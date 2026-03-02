- Projects created with **[[Spring Boot Project Initialization#Spring Initializr|Spring Initializr]]** include essential **starter dependencies** by default
- Starters enable Spring Boot’s core goals: **simplicity** and **auto-configuration**
- A starter dependency = a bundle that pulls in required libs a web app needs
## `spring-boot-starter`
- Core starter with essential Spring Boot components
	- Provides basic [[Bean]] registration, logging, and Spring Context setup
	- Serves as the foundation for all Spring Boot projects
	- Included transitively by other starters
- **Examples of Included Libraries:**
	- `spring-core`
	- `spring-context`
	- `spring-boot-autoconfigure`
	- SLF4J + Logback (default logging system)
## `spring-boot-starter-test`
- A comprehensive set of integrated testing tools for **Spring-based tests**.
- Includes libraries for *unit testing, integration testing, mocking, and more*.
- [[Spring test module - spring-boot-starter-test]] < everything explained here lol

## `spring-boot-starter-web`
- Marks app as a **web app**
- Includes **embedded Tomcat library (`tomcat-embed-core`)**  **transitively** (meaning, it's a dependency of the starter, so you get it automatically when you add the starter) 
## Executable JARs
>Spring Boot projects are, by default, built as **standalone "fat" JARs**.
- Fat JAR
	- a single, executable file that packages your project's code, all its dependency libraries, configuration files, resources, and an embedded web server into one JAR file
	- can be executed using the `java -jar` command
```shell
$ ./gradlew bootJar # Or ./mvnw spring-boot:repackage for Maven
$ java -jar build/libs/myapp-0.0.1-SNAPSHOT.jar
```

**(Reference) Internal Structure:**
```
myapp.jar
├─ BOOT-INF/
│   ├─ classes/       <- Application classes
│   └─ lib/           <- Dependency libraries
├─ META-INF/
└─ org.springframework.boot.loader.Launcher
```
- `spring-boot-loader`
	- - Entry point that separates and loads app classes and libraries at runtime
	- Comes directly from the **Spring Boot project itself**.
	- Not something you manually add as a dependency like `spring-boot-starter-web` => - it's **automatically integrated into your executable JAR** when you build your Spring Boot application using the official **Spring Boot Maven Plugin** or **Spring Boot Gradle Plugin**