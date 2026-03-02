
>[!what is it]
>The *most crucial annotation* in a Spring Boot application. 
>- A *meta-annotation* (a composite annotation) that, with a single declaration, enables *auto-configuration, component scanning, and configuration class registration*.
>- With just this single annotation, hundreds or more [[Bean|Beans]] can be automatically registered
>

- We can know its a spring application w/ this annotation -> It's the entry point to all spring projects
- Spring Boot's package scanning starts from the location of your **main application class** (the one annotated with `@SpringBootApplication`) and scans downwards
	- [[Starting a Spring Boot Application]]
- You should place the main class at root package so all components are detected
- Example
```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

# Diagram
![[SpringBootApplication.png|400]]
- When you put `@SpringBootApplication` on your main class, it's doing three big things at once:
    1. It turns on component scanning (`@ComponentScan`).
    2. It turns on auto-configuration (`@EnableAutoConfiguration`).
    3. It also tells Spring, "Hey, this class itself is a **configuration class**" (which is what `@SpringBootConfiguration` does).
# `@ComponentScan`
> automatic component detection
- `@SpringBootApplication` has `@ComponentScan` internally
- Finds all objects with `@Component` *in your own code* and makes them into a [[Bean]] (registers them in the Spring Container)
	- Basically gets the code written by the devs + make them [[Bean|beans]]
	- components like `@Controller`, `@Service`, `@Repository` all have `@Component` in them
- Normally you don't have to touch this
- **The range is the current folder and all its sub folders**

- You can adjust the scope like this
```java
@SpringBootApplication(scanBasePackages = "com.example.demo")
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}

// same thing
@SpringBootApplication
@ComponentScan(basePackages = "com.example.demo")
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}

// For multiple packages
@SpringBootApplication
@ComponentScan(basePackages = {"com.example.api", "com.example.core"})
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

#### `excludeFilters`
- `@ComponentScan` provides `includeFilters` and `excludeFilters` options to filter targets to be included/excluded
- Filter by *type*, *annotation*, or *expression*

|FilterType|Description|
|:--|:--|
|`ANNOTATION`|Include/exclude classes with specific annotations.|
|`ASSIGNABLE_TYPE`|Include/exclude specific classes or interface implementations.|
|`REGEX`|Filter class names based on regular expressions.|

```java
// 🦄Type
// prevents `MemberDAO.class` from being registered as a Spring Bean, even if it's located within `com.codeit` and annotated with `@Repository` or `@Component`
@ComponentScan(basePackages="com.codeit",
    excludeFilters={
        @ComponentScan.Filter(type=FilterType.ASSIGNABLE_TYPE,
        classes={MemberDAO.class})})
public class ContextConfiguration {}

//🦄Annotation
// **exclude any class** that is annotated with `@org.springframework.stereotype.Component`
@ComponentScan(basePackages="com.codeit",
    excludeFilters={
        @ComponentScan.Filter(type=FilterType.ANNOTATION,
        classes={org.springframework.stereotype.Component.class})})
public class ContextConfiguration {}

// 🦄Expression
// **exclude any class** whose fully qualified name (including its package) matches the given regular expression pattern
@ComponentScan(basePackages="com.codeit",
    excludeFilters={
        @ComponentScan.Filter(type=FilterType.REGEX,
        pattern={"com.codeit.demo.annotationconfig.java.*"})})
public class ContextConfiguration {}

```

#### `includeFilters`
- You can use the `includeFilters` attribute of `@ComponentScan` to *explicitly include certain components* in Bean scanning.

```java
@ComponentScan(basePackages="com.ohgiraffers",
    useDefaultFilters=false,
    includeFilters={@ComponentScan.Filter(type=FilterType.ASSIGNABLE_TYPE,
        classes= {MemberDAO.class})})
public class ContextConfiguration {}
```

- The default value of the `useDefaultFilters` attribute is `true` - it automatically processes and registers annotations like `@Component` and its stereotypes. 
	- If this attribute is set to `false`, component scanning will not occur by default. 
	- You then need to explicitly define the `includeFilters` attribute to enable component scanning for specific targets.
- Note that the method of configuration is the same as for `excludeFilters`, so examples for different types (like `ANNOTATION` and `REGEX`) are omitted
	- When you execute the code below, the `MemberDAO` class will be included in the Bean scanning, and you can confirm the bean with that name.

```Java
ApplicationContext context
    = new AnnotationConfigApplicationContext(ContextConfiguration.class);
String[] beanNames = context.getBeanDefinitionNames();
for(String beanName : beanNames) {
    System.out.println("beanName : " + beanName);
}
```

# `@EnableAutoConfiguration`
> **Activates Spring Boot's auto-configuration**.
- Scans the **classpath**, existing **configs**, and registered **Beans**
- It looks at what libraries you've included in your project (on the "classpath"). Based on those libraries and some default assumptions, it automatically sets up a lot of the common configuration for you in the Spring container.
	- *NOT* getting what the dev wrote (code)
	- Based on these, it **automatically registers** Beans via **predefined auto-config classes**
	- Ex - If you've included a "Web App" dependency (the `spring-boot-starter-web` dependency), `@EnableAutoConfiguration` sees that and automatically  writes tons of configuration code for stuff like standard features like setting up a web server, a database connection, or JSON handling. 
- Spring Boot just **does it for you** because you included the relevant "starter" dependencies 
# `@SpringBootConfiguration`
> An annotation that explicitly marks a class as a **configuration class** within a Spring Boot application (설정 클래스임을 명시.)
- internally serves the same role as `@Configuration`
- This means it's where you can define Java-based manual configurations, register Spring Beans explicitly, or set up various environment configurations.
```java
@Configuration // @SpringBootConfiguration conceptually works like this
public class AppConfig {
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```
- Since it's already in `@SpringBootApplication`, you will almost never type this yourself
- acts as a semantic marker: it explicitly says, "This class is _the_ primary source of configuration for _this Spring Boot application_"