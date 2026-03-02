
>[!AOP (Aspect-Oriented Programming)] 
>A programming paradigm that enables the management of **common functionalities (cross-cutting concerns) separate from the core business logic, by modularizing them into an "Aspect."** 
>- Core concept here is [[Spring's main philosophies#SoC (Separation of Concerns)|Separation of Concerns]]
>- A programming paradigm also used outside of Spring
>- Applying AOP allows defining common functionalities in one place, automatically "woven" into the necessary core logic
>- 관점 지향 프로그래밍

- Limitations of OOP
	- OOP separates responsibilities by roles (classes/modules). 
	- Repetitive logic like logging, security, transactions, and timing often appears across modules. This logic gets scattered and duplicated.
	- OOP *struggles to eliminate such cross-cutting concerns cleanly*
- **Cross-Cutting Concerns (횡단 관심사)**
	- functionalities that are not directly related to business logic but appear repetitively across multiple layers or components => AOP reduces this
	- Examples
		- [[Transaction]]
		- [[Logging]]
		- Security
		- Checking performance
# Example - [[🐣Spring & SpringBoot]]
- [[⭐Layered Architecture#`@Repository`|Layered Architecture - Repository]]
#### Before AOP
```java
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    public void join(Member member) {
        long start = System.currentTimeMillis(); // Non-core logic

        try {
            // Core Logic
            memberRepository.save(member);
        } finally {
            long end = System.currentTimeMillis(); // Non-core logic
            long time = end - start;
            System.out.println("MemberService.join execution time = " + time + "ms"); // Non-core logic
        }
    }
}
```
- Core logic and supplementary logic are mixed within a single source code file.
- The `join()` method contains execution time measurement code mixed with its core logic (saving a member).
- If this logic is repeated across multiple methods or classes, it increases duplication, making maintenance difficult.

#### After AOP
```java
// Core Logic Class
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    public void join(Member member) {
        memberRepository.save(member); // Pure core logic
    }
}
```

```java
// Common functionality (here, logging execution time) separated
@Aspect // Marks this class as an Aspect
@Component // Registers this Aspect as a Spring Component/Bean
public class TimeTraceAspect {

    // Specifies where this advice should be applied (all methods in 'com.example' and its sub-packages)
    @Around("execution(* com.example..*(..))")
    public Object trace(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis(); // Common functionality

        try {
            return joinPoint.proceed(); // Executes the actual target method
        } finally {
            long end = System.currentTimeMillis(); // Common functionality
            long time = end - start;
            // Common functionality
            System.out.println("[Execution Time] " + joinPoint.getSignature() + " = " + time + "ms"); 
        }
    }
}
```
- `@Aspect`
	- Indicates that this `TimeTraceAspect` class is an `Advisor`
- Core logic and supplementary logic can be completely separated.
- Common functionalities are managed in one place, greatly improving maintainability and extensibility.
- `ProceesingJoinPoint joinPoint`
	- Use this where you actually want to execute the business logic code
- `@Around("execution(* com.example..*(..))")`
	- This is where you select the `Pointcut`
- Also the join points (whatever object of the methods u want to AOP) should be a *bean* 
# AOP in Spring
>[!summary]
>Spring supports `@AspectJ` style AOP and primarily provides **proxy-based AOP** from the full AspectJ capabilities.

- `AspectJ` and `Spring AOP`
	- `AspectJ` - A framework that integrated the `Aspect` functions into [[☕Java|Java]]
	- `Spring AOP` - A framework that integrated the `Aspect` functions into [[🐣Spring & SpringBoot|Spring]]
- **Method Join Points Only** 
	- Advice can only be applied at runtime when the core functionality's (target object's) **method** is called.

![](https://howtodoinjava.com/wp-content/uploads/2015/01/spring-aop-diagram.jpg)

| Term                                            | Description                                                                                                                                                                                                                                                                                             | Korean                                                                        |
| :---------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| **Aspect/Advisor (Spring)** (Advice + Pointcut) | A module that defines a cross-cutting concern. <br>- In Spring, this is typically a **separate class** annotated with `@Aspect`.<br>- It contains both the `Advice` (the "what") and the `Pointcut` (the "where").<br>- E.g., logging, transactions.                                                    | 부가 기능 (Aspect) + 적용 위치 (Pointcut)<br>- 어떤 로직을 어디에 지정할지<br>- Aspect 하나 만들면 재사용 |
| **Advice**                                      | The **actual code** that is executed for the cross-cutting concern<br>- The _specific code_ for the extra action (e.g., logging, printing, security, etc. <br>- Also says _when_ to do it (e.g., `@Around` the method).<br>- E.g., `@Before`, `@After`, `@Around`, `@AfterReturning`, `@AfterThrowing`. | 부가 기능<br>- Aspect가 무엇을 언제할지 정의하고 있음                                           |
| **Pointcut**                                    | An expression that **selects** the `Join Points` where the `Advice` should be applied. <br>- Acts as a filter. <br>- E.g., `execution(* com.example..*(..))` is a pointcut that matches all method executions in the `com.example` package and its sub-packages.                                        | Advice에 적용할 JoinPoint를 선별하는 작업/그 기능을 정의한 모듈                                   |
| **Join Point**                                  | *An execution point* in your code where the Advice _could_ be applied<br>- E.g. The function `printA()` where it is defined                                                                                                                                                                             | Advice가 적용될 수 있는 모든 위치                                                        |
| **Weaving**                                     | The process of linking the `Aspect` with the target object to *create the final proxied object*. <br>- In Spring AOP, weaving happens at runtime when the application starts.                                                                                                                           |                                                                               |
- **Proxy**
	- 자신이 클라이언트가 사용하려고 하는 실제 대상인 것처럼 위장해서 클라이언트의 요청을 받아주는 것
	- 사용 목적에 따라
		- **프록시 패턴**: 클라이언트가 타깃에 접근하는 방법을 제어하기 위해서
		- **데코레이터 패턴**: 타깃에 부가적인 기능을 부여해주기 위해서
- **Target Object**
	- 부가 기능을 부여할 대상
- AOP를 구현하는 방법
	- 컴파일 시점에 코드에 공통 기능 삽입
	- 클래스 로딩 시점에 바이트 코드에 공통 기능 삽입
	- 런타임 시점에 프록시 객체를 생성하여 공통 기능 삽입
# How it works
- **Proxy-based AOP implementation**
	- It creates and provides a *proxy* for the target object.
	- At runtime, Spring creates a proxy object that wraps the target object (e.g., your `MemberService`). 
	- When a method on the proxy is called, the proxy can execute the advice  before or after delegating the call to the original target object.
- 런타임시 위빙
	- 스프링이 실행되는 시점에 우리가 정의한 Advice와 핵심 비즈니스 로직을 연결하는 작업
	- 위빙 - 부가 기능과 핵심 기능을 연결하는 작업
	- 런타임 - 스프링이 실행되는 시점
- 원래는 Spring이 실행되면 바로 빈으로 등록됨
- 하지만 AOP를 적용하면
![[spring-aop.png]]
- 전체 흐름
	- 먼저 AOP를 적용하면, spring이 객체를 만들고 컨테이너에 넣지 않고 빈 후처리기에 넣음
	- 빈 후처리기는 Aspect Advisor를 조회하면서 아까 정의한 `Advisor` (`@Aspect` 어노테이션)을 다 가져옴
	- `Advisor`목록을 하나씩 확인한 후 아까 넣은 객체가 proxy의 대상인지 아닌지 확인
		- 이전에 지정했던 Pointcut을 확인
		- `HelloService`(예시)에는 Pointcut으로 지정한 메서드가 포함됨 -> Advisor의 Pointcut 대상이 됨
		- 이 Pointcut을 advice에 가져와서 그거를 적용한 proxy가 생성이 됨
	- 이제 이 proxy는 스프링 컨테이너에 등록됨
	- 이제 빈 객체를 호출했을 때 proxy를 호출해서 부가 기능이 먼저 실행되고 그 다음에 객체가 호출됨
- 결론: Spring AOP는 AOP 패러다임을 적용하기 위해, 빈 후처리기에서 빈의 프록시 객체를 생성한 뒤 스프링 컨테이너에 등록
# 기억할 것
- Spring AOP에서 빈(Bean)이 생성될 때, 해당 클래스 내의 **단 하나의 메서드라도** Pointcut 조건에 맞으면, **그 객체(빈) 전체가 프록시 객체로 감싸짐** (예. `print`로 시작하는 메서드)
	- 프록시는 그럼 모든 요청을 가로채지만, pointcut이 아닌 메서드면 그냥 원본 객체의 메서드 실행 (pointcut이면 advice 실행 -> 원본 메서드 실행)
- Weaving(위빙)이란 무엇이며, Spring AOP는 어떤 시점에 위빙을 수행하나요?
	- Weaving은 원본 코드(Target)에 부가 기능(Aspect)을 합쳐서 프록시 객체를 만드는 과정을 말합니다. 컴파일 시점이나 클래스 로드 시점에 하는 방식(AspectJ)도 있지만, Spring AOP는 '런타임(Runtime) 시점'에 프록시 객체를 생성하여 위빙을 수행합니다. 따라서 실제 코드를 변경하지 않고 실행 중에 대리자(Proxy)를 통해 부가 기능을 제공합니다.