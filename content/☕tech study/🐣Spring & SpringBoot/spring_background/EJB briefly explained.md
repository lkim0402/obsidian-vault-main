
>[!Enterprise JavaBeans]
>An early, complex Java EE specification (framework) for building distributed, enterprise-level applications.

- **Original Goal:** Simplify enterprise development by handling common concerns like transactions, security, and concurrency automatically.
- **Why it became "bad":**
    - **Heavyweight & Complex**
	    - Used XML, needed to make lots of files just for simple logic
	    - Required a lot of configuration and generated code, making development cumbersome.
    - **Vendor Lock in**
	    - Required specific application servers (WAS like WebLogic, WebSphere), leading to vendor lock-in.
		    - if you changed one aspect, you needed to change everything else too
    - **Slow Deployment**
	    - If you changed one aspect, you had to change numerous locations, making it slow for deployment
	- **Difficult to test, not POJO**
	    - EJB components had to extend specific EJB classes or implement interfaces, making them dependent on the EJB container and hard to test outside it.
	    - [[POJO vs JavaBeans VS Spring Beans]]
		- [[Spring]] easily integrates with JUnit or Mockito, enabling tests without containers 
	- **Violated OOP - Tight coupling**
		- had to create the instance in the class, enabling tight coupling
```java
// example of tight coupling
public class CustomerBean {
    // 결합도 ↑
    private CustomerRepository repo = new CustomerRepository(); 
}
```
-   이렇게 EJB 내부에서 직접 객체 생성을 함으로서 테스트나 재사용에 제약이 많았다. 
	-  `CustomerRepository`의 **생성자 시그니처가 변경되면**, 이를 사용하는 **모든 클래스에서 일일이 수정을 해야 함**.
	-  `CustomerRepository`를 **Mock이나 다른 구현체로 대체할 수 없어**, 단위 테스트 작성이 힘듦.
# Spring
- The **Spring Framework** emerged as a lightweight, flexible, and testability-centric alternative to the complex and rigid Java EE
```java
@Service
public class CustomerService {
    private final CustomerRepository repo;

    public CustomerService(CustomerRepository repo) {
        this.repo = repo; // 외부에서 주입받음 → 테스트 용이
    }
}
```
# Summary
- **EJB's Flaws:** Enterprise JavaBeans (EJB) in Java EE offered complex features (transactions, security) but suffered from excessive configuration, high vendor lock-in, and poor testability.
- **Java EE Dissatisfaction:** Developer frustration with Java EE's rigid structure spurred a search for simpler, more flexible alternatives.
- **Rod Johnson's Solution:** Rod Johnson, in his book, criticized Java EE and proposed the need for a lightweight framework to address its issues.
- **Spring's Rise:** Born from this criticism, the Spring Framework revolutionized Java development by focusing on POJO-centric design, Dependency Injection (IoC), Aspect-Oriented Programming (AOP), ease of testing, and container independence.






