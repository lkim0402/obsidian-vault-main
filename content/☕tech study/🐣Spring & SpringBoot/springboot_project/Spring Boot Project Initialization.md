# Spring Initializr
https://start.spring.io/
- Spring Boot automates many tasks, but **initial setup** (deps, structure) still needs definition
- **Spring Initializr** helps generate a ready-to-use project with selected dependencies
	- The options you select here actually influence the entire `build.gradle` or `pom.xml`, `application.properties`, and the overall `src` structure.
- You can also just use an Ide
	- 메뉴: File > New > Project > Spring Initializr + follow the info (metadata) 

| Item                 | Description                                              |
| :------------------- | :------------------------------------------------------- |
| **Project**          | Select Maven or Gradle                                   |
| **Language**         | Java (or Kotlin, Groovy)                                 |
| **Spring Boot**      | Version selection available                              |
| **Project Metadata** | Enter Group, Artifact, Name, Package Name, etc.          |
| **Dependencies**     | Select desired Starters (`Web`, `JPA`, `Security`, etc.) |
# Project metadata
![[spring-metadata.png|400]]
- Group (`groupId`)
	- Org or project group ID, follows reverse domain
	- Ex: Company _Ohgiraffers_’s `abc` project → `com.ohgiraffers.abc`
	- Ex: Organization _Spring_’s `xyz` project → `org.spring.xyz`
- Artifact (`artifactId`)
	- Project/module name within group
	- Becomes the JAR/WAR name (e.g., `user-service`)
- Name
	- Human-readable project name (e.g., `User Service API`)
	- Used in docs/IDEs
- Package Name
	- Base Java package for code (e.g., `com.mycompany.myapp.userservice`)
	- Often = Group + Artifact
# Java version
- Choose java version
	- Java 17+ recommended** (Spring Boot 3.x+ requires Java 17)
	- JDK version must match build tool config (e.g., Gradle) to avoid compile errors
# Jar vs. War (packaging method)
- Jar (Default)
	- Standard choice unless special needs
	- Creates executable single JAR → run with `java -jar`
	- Includes embedded Tomcat
	- ✅ Fits most projects (MSA, cloud, Docker)
- War
	- Traditional WAR deployment
	- Requires external WAS (Tomcat, JBoss, etc.)
	- Used mainly for legacy system integration

# Spring Boot version
- Generally use the **Default (Stable)** version
- Use **Snapshot/Beta** only for testing new features or preview APIs
- ⭐ **All team members must use the same Spring Boot version**  
    → Prevents build errors and config mismatches


