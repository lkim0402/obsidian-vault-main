
>[!Inversion of control]
>An architectural concept where the control flow of a program, including object creation, initialization, and execution, is **handled by an external system (a container)** rather than being directly managed by the developer. 
>- The concept of IoC is frequently encountered in frameworks.
>	- [[Library VS Framework]]

- IoC => When using this principle, instead of allowing the app to control the execution, we give control to the [[Spring Framework & Ecosystem|Spring Framework]]
- diagram
	![[IoC-diagram.png]]
# IoC in Spring Framework
>IoC is a design principle, and frameworks like Spring are built _on top_ of this principle.
- The Spring Framework's **IoC container** is what performs the "inversion." 
	- It's the central part of Spring that takes over the responsibility of creating your objects, configuring them, and injecting their dependencies.
- Spring dictates the "flow"
	- You provide components and the framework decides when and how to instantiate & connect them
	-  Basically you *give up the right to use the keyword `new`*
	- You simply tell Spring (via configuration): "Hey, I need a `UserService`." Spring's reaction: "Okay, I see `UserService` needs a `UserRepository`. I will create the repository, I will create the service, I will connect them, and I will have them ready for you."
## Without IoC
```java
// Repository
public class MemberRepository {
    public String findMemberName(Long id) {
        return "김스프링";
    }
}

// Service
public class MemberService {
    private MemberRepository memberRepository;

    public MemberService() {
        // Directly creating the dependent object -> tight coupling
        this.memberRepository = new MemberRepository();
    }

    public String getMemberName(Long id) {
        return memberRepository.findMemberName(id);
    }
}

// Controller
public class MemberController {
    private MemberService memberService;

    public MemberController() {
        // Directly creating the dependent object -> tight coupling
        this.memberService = new MemberService();
    }

    public void printMember(Long id) {
        String name = memberService.getMemberName(id);
        System.out.println("회원 이름: " + name);
    }
}
```
- **The developer is in control** 
	- The developer explicitly writes `new MemberRepository()` and `new MemberService()`. They are manually managing the creation and dependencies.
- **Strong coupling** 
	- Each object explicitly depends on the concrete implementation of the object it uses. 
	- If `MemberRepository` changes its constructor (maybe we changed it to need parameters), `MemberService` would also need to be updated. 
	- If you wanted to use a different `MemberRepository` implementation for testing, you'd have to change the `MemberService` code. (hard to swap for mock)
## With IoC
```java
// Repository
@Repository
public class MemberRepository {
    public String findMemberName(Long id) {
        return "김스프링";
    }
}

// Service
@Service
public class MemberService {
    private final MemberRepository memberRepository;

    // Spring automatically (and externally) injects + constructor injection -> advantageous for testing and extension
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    public String getMemberName(Long id) {
        return memberRepository.findMemberName(id);
    }
}

// Controller
@RestController
public class MemberController {
    private final MemberService memberService;

    // Spring automatically (and externally) injects + constructor injection -> advantageous for testing and extension
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @GetMapping("/members/{id}")
    public String getMember(@PathVariable Long id) {
        return memberService.getMemberName(id);
    }
}
```
- Developers simply **define roles (interfaces) and provide configuration information** (annotations or settings), and **Spring then handles the entire control flow**.
- With **IoC**, control over objects is delegated to a **container**. This means the [[Library VS Framework#Framework|framework]] (like Spring) takes on the role of creating and managing objects and connecting dependent objects.


# IoC Container (aka. Spring Context)
>[!IoC Container]
>Refers to a ***concrete framework* that implements Inversion of Control**.
>- A central management unit that creates objects (Beans) used in an application, injects dependencies, and manages their lifecycle
>- Using an IoC Container allows for the automatic creation, initialization, and dependency handling of objects.
>- A prime example of an IoC Container is Spring Framework's `ApplicationContext`.

- Spring provides various IoC Container implementations, but the most commonly used one is `ApplicationContext` (and `BeanFactory` ).
- The main role of the Spring IoC Container is to **manage the entire lifecycle of [[Bean]] objects**, from **creation, management, and configuration to destruction.**
- Using the IoC container, to which you often refer as the Spring context, *you make certain objects known to Spring, which enables the framework to use them in the way you configured*

## `ApplicationContext`
![[ApplicationContext.png]]
>[!ApplicationContext]
>A core IoC Container implementation in Spring, extending `BeanFactory`
>-  `BeanFactory`'s ability to register and manage [[Bean|beans]] +  numerous additional features offered by Spring
>- This is why `ApplicationContext` is the most commonly used in real-world applications 
>- It's an interface

- It adds the following functionalities:
	- **`ListableBeanFactory`**: Includes all the capabilities provided by `BeanFactory`.
	- **`ApplicationEventPublisher`**: Provides event handling capabilities.
	- **`MessageSource`**: Supports internationalization (i18n) by resolving messages.
	- **`ResourceLoader`**: Provides resource handling capabilities.
	- AOP integration
	- BeanPostProcessor (Bean Post-processor) support
	- Automatic Bean registration support (e.g., through component scanning)

## Implementations

| 구현체 이름                               | 설명                                                                                                               | 특징                      |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `AnnotationConfigApplicationContext` | Java 기반 설정용<br>- reads **Java-based Configuration Metadata** to perform its container role                       | `@Configuration` 클래스 기반 |
| `GenericWebApplicationContext`       | Spring Web 환경용<br>- reads **XML-based Configuration Metadata** to perform its container role<br>- deprecated lol | Spring MVC 및 Boot의 기반   |
| `WebApplicationContext`              | Spring MVC의 특수 컨텍스트                                                                                              | DispatcherServlet과 연계   |
| `ClassPathXmlApplicationContext`     | XML 설정용                                                                                                          | 과거 버전과 호환, 지금은 잘 안 씀    |
| `ConfigurableApplicationContext`     | (sprint과제 때 사용)                                                                                                  |                         |
```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

MyService service = context.getBean(MyService.class);
```

- You can use `.getBean()` to inspect specific bean, but this violates [[IoC (Inversion of Control)]], [[DI (Dependency Injection)]] , and  [[DIP (Dependency Inversion Principle)|DIP]] so we don't use it
	- we need to be able to use the bean without knowing its name
	- but if we use `getBean()` we need to know the bean
	- we can use getBean through:
		- its type (`UserService.class`)
		- By its name (less common when you have an interface and a single implementation, but possible).
After `refresh()`, its state can be inspected through the following key fields:

|Item|Description|
|:--|:--|
|`beanFactory`|The core object responsible for **Bean creation and dependency injection**.|
|`environment`|Contains **environmental information**, such as active profiles and configuration values.|
|`applicationListeners`|A list of **registered application event listeners**.|
|`messageSource`|Provides **message resources for internationalization (i18n)**.|
|`lifecycleProcessor`|A component that **manages the application's startup and shutdown**.|
## `BeanFactory`
>The most fundamental interface in Spring's IoC Container hierarchy
- Provides only the minimal functionality to create and return [[Bean|beans]]
- Was used in early Spring versions but is **rarely used now**
- Supports **lazy initialization** (lazy loading)
	- this is a key difference from `ApplicationContext`
	- 생성시점, 초기화 시점이 나눠짐
- Centered around explicit Bean lookup via `getBean()` method.
- bean 최소 기능 명세만 제공 (interface니까)
## Configuration Metadata
![[IoC-configuration-metadata.png|400]]
- Refers to the **configuration information** that tells the IoC Container _which_ objects to create, _how_ to create them, and _which_ dependencies to inject. 
	- Think of it as the "work instruction manual" for the IoC Container, detailing what work it needs to do and how to do it
- **Three ways to provide this Configuration Metadata**:
	- (While Java Config is effectively the standard, in practice, a hybrid approach combining Java Config with Annotation-based configuration is widely used.)

| Method                              | Recommendation               | Description                                                        |
| :---------------------------------- | :--------------------------- | :----------------------------------------------------------------- |
| **XML-based**                       | ❌ Discouraged                | Explicit but cumbersome and lacks type safety.                     |
| **Java Config (`@Configuration`)**  | ✅Recommended                 | Explicit, ensures type safety, and is the default for Spring Boot. |
| **Annotation-based (`@Component`)** | ✅Recommended (as supplement) | Effective when used in conjunction with Java Config.               |

# Object Lifecycle Management
#### Bean Lifecycle Phases
>The IoC container doesn't just create objects; it manages the **entire lifecycle** of each object. 
- Allows devs to focus on core business logic without intervening in the object's creation, initialization, or destruction.
1. **Loading Bean Definitions** 
	- The container reads metadata for each bean from Spring configuration files (XML or annotation-based). This information includes the class type, dependencies, initialization methods, and all other details necessary for bean configuration.
2. **Object Instance Creation**
	- The *bean class object is instantiated* using the `new` keyword. Dependency injection has not yet occurred at this stage.
3. **Dependency Injection** 
	- The container *injects other beans that the current bean requires*. 
	- Various injection methods can be configured, such as constructor, setter, or field-based injection.
4. **Initialization Callbacks Execution** 
	- Bean initialization logic can be defined and executed in the following ways:
	    - Methods annotated with `@PostConstruct`.
	    - The `afterPropertiesSet()` method of the `InitializingBean` interface.
	    - XML configuration using the `<bean init-method="..." />` attribute.
5. **In Use**
	- Once initialized, the bean is managed by the container and can be freely used within the application logic. 
	- From this point, the bean, with its dependencies injected, *performs its actual business functionality*.
6. **Destruction Callbacks Execution** 
	- These methods are called *before the container shuts down or the bean is destroyed*, allowing for *resource cleanup* or other *pre-shutdown tasks*:
	    - Methods annotated with `@PreDestroy`.
	    - The `destroy()` method of the `DisposableBean` interface.
	    - XML configuration using the `<bean destroy-method="..." />` attribute.