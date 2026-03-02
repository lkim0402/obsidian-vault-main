
>[!Gradle]
>Your project's **build automation tool**
>- program that **automatically handles all the processes required to transform our code into an executable form** (like JAR or WAR files)
>- **Handles the libraries (modules) that have [[DI (Dependency Injection)#Dependency|dependencies]]

- Spring Boot makes it super easy to get started with Gradle because Initializr sets everything up, and then you just use `build.gradle` to define your project and `./gradlew` to execute commands.
- You can actually click the right elephant icon on the menu in Intellij, or type `./gradlew build` in the terminal!
	- **`build`:** This is a general-purpose task. When you run it, it compiles your code, runs tests, and creates a `JAR` file. You noted that it creates a "plain" JAR (without any dependencies)
	- **`bootJar`:** This is a specific Spring Boot task. It creates an executable "fat" JAR. This JAR file contains all the necessary dependencies, so you can run your application with a simple `java -jar` command
# What is gradle
#### Build
> A sequence of operations that make up the process of creating a deployable application
- **Compile**:  `.java` -> `.class`.
- **Resolve Dependencies**: Downloading necessary external libraries (e.g., PostgreSQL, Lombok).
- **Package**: Bundling `.class` files, resources, etc., into a single `.jar` or `.war` file.
	- jar 은servelet container을 포함해서 바로 실행가능
	- war은 수동적으로 톰캣같을걸 설치해야 실행 가능함
- **Test**: Executing test code and verifying results.
- **(Optional) Deploy**: Distributing the built application to a server.
	- Gradle doesn't include this (included only for general definition of build)
	- Tools like github actions
	- CI/CD
#### Gradle's Role in Spring Boot
- **Main options:** Gradle or Maven
- **Choosing Gradle (via Spring Initializr)** auto-generates key Gradle files for build management
- In Spring Boot, it manages environment setup, dependencies, builds, tests, and app launch
	- `./gradlew build` creates a full project package without needing an IDE

| File            | Description                                     |
| --------------- | ----------------------------------------------- |
| build.gradle    | Core build config and dependencies              |
| settings.gradle | Top-level project settings (e.g., project name) |
| gradlew         | Gradle Wrapper for Linux/macOS                  |
| gradlew.bat     | Gradle Wrapper for Windows                      |
| gradle/         | Wrapper config files                            |
# `build.gradle`
- When you set up a Spring Boot project with Gradle, the `build.gradle` file is automatically generated
- Very important -> Defines project execution environment & declares plugins, dependencies, and build tasks

## Plugin config
By default, Gradle doesn't know if your project is a Java project or a Spring project. You extend its capabilities by applying **plugins**.
```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.4'
    id 'io.spring.dependency-management' version '1.1.4'
}
```

|Plugin|Purpose|
|---|---|
|`java`|Enables Java compilation, testing, and packaging|
|`org.springframework.boot`|Adds Spring Boot features like `bootRun` and `bootJar` for running and packaging the app|
|`io.spring.dependency-management`|Manages library versions via Spring Boot’s BOM, ensuring compatibility and avoiding conflicts|

## [[DI (Dependency Injection)#Dependency in Gradle|Dependency]] Configuration
> Any libraries you declare here will be automatically downloaded by Gradle and included in your project
- The project has dependencies to other libraries/modules
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
- actually you put a version after : (last part) but `gradle` handles that for you!
	- Spring Boot's dependency management (via the `io.spring.dependency-management` plugin and the BOM) intelligently manages versions for you when using starters
- The keywords like `implementation`, `testImplementation` are called *Gradle dependency configurations*

| Gradle Configuration      | Purpose                                                                           | When to Use                                                                             |
| ------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **`implementation`**      | For dependencies needed to compile and run your code.                             | Use for libraries like Spring Web or JPA that your code directly references.            |
| **`runtimeOnly`**         | For dependencies only needed when the application runs, not for compilation.      | Use for database drivers (e.g., PostgreSQL) or logging implementations (e.g., Logback). |
| **`testImplementation`**  | For dependencies needed to compile and run your tests.<br>- [[Testing in Spring]] | Use for testing frameworks like JUnit, Mockito, or AssertJ.                             |
| **`testRuntimeOnly`**     | For dependencies only needed to run your tests, not to compile them.              | Use for an in-memory database driver like H2, when it's only for testing purposes.      |
| **`annotationProcessor`** | For libraries that generate code at compile time.                                 | Use for tools like Lombok or MapStruct that process annotations and generate classes.   |

#### Advantages
- **Automatic dependency download**
	- libraries are downloaded automatically, so you don't have to manually find and add them
- **Automatic Transitive Dependency Resolution**
	- If a library you use relies on other libraries, those "sub-dependencies" are automatically pulled in too
- **Version Conflict Resolution**
	- helps *sort out conflicts* when different libraries require *different versions of the same dependency*
- **Automatic Security Updates and Compatibility**
	- Staying updated with security patches and ensuring compatibility across libraries becomes much easier

- Example: Simply adding this one line to your `build.gradle` will automatically pull in **dozens of related libraries** internally, saving you from having to list each one individually.:
```java
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

| Configuration        | Description                                                             | Example Use Case                  |
| -------------------- | ----------------------------------------------------------------------- | --------------------------------- |
| `implementation`     | Needed for both *compile* and *runtime*; hides transitive deps from API | Most libraries your app uses      |
| `developmentOnly`    | Exclusive to the *development environment*                              | Spring DevTools                   |
| `testImplementation` | Only for **test** *compile and runtime*; excluded from final build      | Testing frameworks like JUnit     |
| `compileOnly`        | Needed *only at compile time*, not runtime                              | Annotation processors like Lombok |
| `runtimeOnly`        | Needed *only at runtime*, not compile time                              | JDBC drivers                      |
- developmentOnly은 Spring Boot Gradle Plugin이 제공하는 별도 configuration임
#### Plugin
```groovy
plugins {  
    id 'java'  
    id 'org.springframework.boot' version '3.5.0'  
    id 'io.spring.dependency-management' version '1.1.7'  
}
```
- The `io.spring.dependency-management` plugin works with Spring Boot's **BOM (Bill of Materials)**.
	- this plugin tells Gradle: "For all the Spring-related libraries in this project (dependencies listed in `build.gradle`), here's a **master list of compatible versions** that are known to work well together.
	- plugin **automatically handles most versioning** for you

#### Advanced
- [Notion Link (kor)](https://calm-individual-12a.notion.site/210c6b7098288153a704d71548a23b0e)
## BOM (Bill of Materials) Management
>A BOM is a **POM file** that centrally manages the version information for a set of dependencies.

#### POM vs BOM

|Feature|**POM (Project Object Model)**|**BOM (Bill of Materials)**|
|---|---|---|
|🔧 **Purpose**|Defines a _single_ project’s build setup|Manages _versions_ across related libraries|
|📁 **File**|`pom.xml`|Also `pom.xml`, but with `<packaging>pom</packaging>`|
|⚙️ **Includes**|Dependencies, plugins, build rules|Version info via `<dependencyManagement>`|
|🧩 **Use Case**|Configure how _your_ project is built|Share consistent versions across modules|
|✅ **In Gradle?**|Not used directly|Supported via `io.spring.dependency-management` plugin

#### `spring-boot-dependencies`
- Spring Boot provides a BOM called `spring-boot-dependencies`. 
	- When developers declare only a [[Spring Boot Starter|Starter]], the corresponding libraries with their pre-defined versions are automatically *mapped* through this BOM.
- Advantages
	- **Consistent Versions**: Prevents conflicts across [[Spring Boot Starter|starters]]
	- **Auto Versioning**: No need to manually specify versions
	- **Easy Upgrades**: Update BOM → all related libraries upgrade
	- **Security Patches**: BOM includes patched versions of libraries
- In Spring Boot, all [[Spring Boot Starter|Starters]] commonly depend on the `spring-boot-dependencies` BOM. This BOM includes:
	- Key Spring Framework modules (e.g., `spring-core`, `spring-context`).
	- Third-party libraries (e.g., Jackson, Tomcat, Hibernate).
	- Testing frameworks (e.g., JUnit, Mockito, AssertJ).

## Task
###### Task Configuration
Gradle builds are organized into various **tasks**. Each task represents a single step in the build process. You can create your own tasks or extend existing ones.
```groovy
tasks.named('test') {
    useJUnitPlatform()
}
```
##### Executing and Verifying Gradle Tasks
Common gradle tasks:

| Command             | Main Function                                      | Runs Tests? | Creates JAR?                     | Runs App? |
| ------------------- | -------------------------------------------------- | ----------- | -------------------------------- | --------- |
| `./gradlew build`   | Full build (compiles, tests, and packages app)     | ✅ Yes       | ✅ Yes (standard JAR or boot JAR) | ❌ No      |
| `./gradlew bootJar` | Only creates an executable Spring Boot JAR         | ❌ No        | ✅ Yes (bootable JAR)             | ❌ No      |
| `./gradlew bootRun` | Runs the app using Spring Boot (embedded server)   | ❌ No        | ❌ No                             | ✅ Yes     |
| `./gradlew run`     | Runs a general Java app (not Spring Boot specific) | ❌ No        | ❌ No                             | ✅ Yes     |
- `./gradlew test` - Executes only the test code.
- `./gradlew dependencies` - Shows the dependency tree for your project.
- `./gradlew tasks`- Displays a list of all available tasks in your project.

- `./gradlew build` process
	1. **`clean`**: Removes any existing build output.
	2. **`compileJava`**: Compiles your Java source files.
	3. **`processResources`**: Includes files from your `resources/` folder.
	4. **`classes`**: Generates `.class` files.
	5. **`test`**: Executes all your test code.
	6. **`bootJar`**: Creates the executable `.jar` file (this is a Spring Boot-specific task).
	- The final executable `.jar` file will be found in the `build/libs/` folder, named as `your-project-name-version.jar`.

# Gradle Wrapper
> When you download a Spring Boot project or clone it from [[Git]], you'll typically run commands like `./gradlew build` or use your IDE's build button. The `gradlew` you're using here isn't Gradle itself, but rather a script called the **Gradle Wrapper**.

- **Gradle Wrapper**
	- an auto-installation script that allows you to build a project **without needing to install Gradle locally**
## Components of the wrapper
When you create a Spring Boot project with Gradle, the following wrapper files are automatically included:
```
gradlew                 <- Executable for Unix-like systems (e.g., ./gradlew)
gradlew.bat             <- Executable for Windows systems
gradle/wrapper/
├── gradle-wrapper.jar  <- The loader for executing Gradle
└── gradle-wrapper.properties <- Defines the Gradle version to use
```
- `gradle-wrapper.properties`
	- most important
	- Specifies the exact Gradle version for the project
	- Ensures consistent Gradle version across all environments, automatically downloads the specified Gradle version if not present
## WHY?
|Problem|How the Gradle Wrapper Solves It|
|---|---|
|Teams use different Gradle versions|Ensures consistent Gradle version auto-installed and used|
|Gradle needs separate installation|Automatically installs Gradle when running the wrapper script|
|Version consistency hard in CI/CD|Pins the Gradle version for reliable, repeatable builds in CI/CD|
## TIPS
- You **must commit** `gradlew`, `gradlew.bat`, and the `gradle/wrapper/` directory to Git.
- Conversely, directories like `.gradle/`, `build/`, and `.idea/` (IntelliJ project files) should **not be committed** and should be included in your `.gitignore` file.
- If you need to **change the Gradle version** for your project, use the following command:
	- `./gradlew wrapper --gradle-version 8.5`