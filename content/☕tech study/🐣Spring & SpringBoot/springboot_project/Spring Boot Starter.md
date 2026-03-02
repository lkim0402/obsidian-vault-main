
>[!Starters]
>A *meta-dependency (메타 의존성)* that bundles together a *prescriptive set of dependencies* needed to implement a specific feature 
>- [[Gradle#DI (Dependency Injection) Dependency in Gradle Dependency Configuration|Gradle Dependency  Configuration]], also look at the [[Gradle#Advantages|advantages of using gradle]]
>- [[DI (Dependency Injection)#Dependency|What is dependency]] (+ [[DI (Dependency Injection)#Dependency in Gradle|Dependency in Gradle]])
- [[🐣Spring & SpringBoot]]
- When you add a single starter, it automatically includes all the related libraries and default configurations associated with that particular functionality
- Example:  `spring-boot-starter-web`
	- [[⭐Spring MVC]]
	- Jackson (JSON 직렬화)
		- A library for our convenience, has `ObjectMapper` class
			- Conversion between [[☕Java]] objects and [[JSON]]
		- Related to broader concept: [[Java Serialization (and persistence)]]
	- Embedded Tomcat
	- Logging (SLF4J + Logback)
- Spring Boot starters are a feature that *maximizes* the [[Gradle#Advantages|advantages of using gradle]].
	- ALSO READ: [[Gradle#BOM (Bill of Materials) Management|Gradle - BOM (Bill of Materials) Management]]
	- All starters rely on BOM
# Maven Central
> Website to explore Spring Boot Starter dependencies in a Gradle environment
[Maven Central](https://central.sonatype.com/artifact/org.springframework.boot/spring-boot-starter-web/dependents)
- *Overview*
	- Contents Included:
		- Group ID / Artifact ID
		- Latest version information
		- License
		- Metadata such as release date and tags
	- How to Use It:
		- When you want to quickly understand the purpose of this library.
		- When you need a summarized view of an unfamiliar artifact's information.
- *Versions*
	- lists **all available versions of an artifact**.
	- Contents Included:
		- Version number
		- Release date
		- Each version is clickable, leading to its detailed information page.
	- How to Use It:
		- To see what versions of the artifact exist.
		- To help decide which version to use, whether you need a stable release or the latest one.
- *Dependents*
	- List of Projects that uses the current artifact as dependencies
	- "Who depends on me?"
	- U can gauge the **popularity or adoption** of a library. 
- *Dependencies*
	- List of projects that the current artifact uses as dependencies
	- Includes **transitive dependencies** (both direct and indirect dependencies are shown).
	- "Who do I depend on?"
	- You can use it to understand the **internal composition** of a library, including its underlying components.

# Most used starters
There are a LOT (50+), but these are the most used.

| Starter Name                     | Description                                                                                                                                |
| :------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| `spring-boot-starter`            | The **minimal common starter**, including basic logging (SLF4J + Logback).                                                                 |
| `spring-boot-starter-web`        | For developing **Spring MVC-based web applications**. (more below)<br>- already includes `spring-boot-starter`                             |
| `spring-boot-starter-data-jpa`   | Integrates **Spring Data JPA with Hibernate** for data access.                                                                             |
| `spring-boot-starter-security`   | Provides **Spring Security functionalities** for authentication and authorization.                                                         |
| `spring-boot-starter-test`       | A comprehensive starter for **testing**, including JUnit, Mockito, and AssertJ.                                                            |
| `spring-boot-starter-thymeleaf`  | Integrates the **Thymeleaf template engine** for server-side [[HTML]] rendering.                                                           |
| `spring-boot-starter-validation` | Integrates the **[[Bean]] Validation API** (typically with Hibernate Validator) for data validation.<br>- Includes validation for [[Bean]] |
| `spring-boot-starter-logging`    | Provides **default logging** based on SLF4J and Logback. (Often included by other starters like `web`).                                    |
| `spring-boot-starter-actuator`   | Exposes **operational metrics and monitoring APIs** for your application.                                                                  |
- You typically combine starters based on your application's structure and role. For example:
	- [[REST API]] Server: You'd likely use starters like `web`, `data-jpa`, `validation`, and `actuator`.
	- Admin Dashboard: You might opt for `thymeleaf`, `security`, and `web` starters.
- **Starters sit at the top** of the Gradle dependency tree, and often **include other starters** or libraries under the hood.
	- Use `./gradlew dependencies` to:
	    - Visualize the full dependency graph.
	    - See which libraries are pulled in.
    - Understand starter hierarchy.
# More about `spring-boot-starter-web`
#### Main Included Dependencies

| Dependency                                  | Description                                                                   |
| ------------------------------------------- | ----------------------------------------------------------------------------- |
| **`spring-web`**                            | Core features of the [[⭐Spring MVC]]                                           |
| **`spring-webmvc`**                         | Provides components like `DispatcherServlet`, `Controller`, `View`, etc.      |
| **`jackson-databind`**                      | Supports JSON serialization/deserialization                                   |
| **`validation-api`, `hibernate-validator`** | Supports [[Bean]] Validation (e.g., `@Valid`)                                 |
| **`spring-boot-starter-tomcat`**            | Runs the application with an *embedded Tomcat server*<br>- [[Spring Servlet]] |
#### The Auto-Configuration Logic
- Once the starter brings in all those "ingredients," the **`spring-boot-autoconfigure`** library kicks in (which is automatically brought in by starters). It looks at the libraries you have available (on your "classpath") and uses conditional logic to set everything up.
- It essentially follows a recipe like this:
	- "**IF** `spring-webmvc` is present, **THEN** I will automatically set up a `DispatcherServlet` and everything needed to make it work."    
	- "**IF** `jackson-databind` is present, **THEN** I will automatically set up the `MappingJackson2HttpMessageConverter` to handle JSON."
    - "**IF** an embedded server like `tomcat` is present, **THEN** I will start it up and configure it to run the application."