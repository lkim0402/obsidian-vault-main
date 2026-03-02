
>[!Overview]
>Testing prevents regressions and speeds up development. Spring Boot’s built-in testing tools allow fast, reliable, and isolated checks without the inefficiency of manual methods like Postman.

- `@Displayname` 
	- use this to explain what the test is doing
- add `given-when-then` comments
	- [[Test-Driven Development (TDD)#Need to know BDD (Behavior-driven development)|Test-Driven Development (TDD) - BDD (Behavior-driven development)]]
# Why
- Manual testing comes with several inconveniences:
	- You have to start the server and open Postman every time, which is tedious.
	- As the number of test cases grows, the repeated work becomes overwhelming.
	- It’s difficult to test specific functions (e.g., a single method) in isolation.

|Category|Manual Testing|Automated Testing (e.g., Unit Tests)|
|---|---|---|
|Execution Method|Developer manually uses Postman, etc.|Click a button in the IDE or run automatically via CI|
|Repeatability|Inefficient, time-consuming|Can be repeated quickly|
|Scope of Verification|Broad (focused on checking overall flow)|Precise verification down to individual functions|
|Productivity|Low (burdensome when repeated)|High (done with a single click)|
|Refactoring Safety|Low|High|
# Types of testing in Spring
## Diagram
![[spring-testing.png|900]]
## Types of tests

| Test Type                     | Main Purpose                                                                                                                                                                              | Test Scope  | Characteristics                                                                                                               | Tool/Annotation                                                                                                                | Code examples                                                          |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| [[Unit Test]] (단위 테스트)        | Verify functionality of small units (methods, classes)                                                                                                                                    | Very narrow | **No Spring Context loaded**. Extremely fast, completely isolated from other parts of the application.                        | - [[JUnit 5]]<br>- [[Mockito]]<br>- `AssertJ`                                                                                  | - [[Example - service unit testing]]<br>                               |
| [[Slice Test]]                | Test specific layers in [[⭐Layered Architecture\|layered architecture]] (e.g., Controller, Repository) in *isolation*                                                                     | Medium      | **Loads a partial Spring Context**. <br>Fast, but slower than a unit test. <br>Uses mocks for dependencies outside the slice. | - `@WebMvcTest` (Controller)<br>- [[Data Access (Repository) Layer Testing#`@DataJpaTest`\|@DataJpaTest]]<br>- [[Mockito]]<br> | - [[Slice Test#Example (Controller slice)\|Example - controller test]] |
| [[Integration Test]] (통합 테스트) | Verify *interactions between multiple layers* including database and services                                                                                                             | Broad       | **Loads the full Spring Context**. Slower, as it involves real components, network calls, and database connections.           | `@SpringBootTest` `TestRestTemplate`<br>`@Transactional` (for DB tests)                                                        |                                                                        |
| [[Functional Test]] (기능 테스트)  | Verify the application behaves correctly from the user/client perspective, testing multiple components<br>- [[Backend & Main Components Explained#Client-side vs Server-side\|client vs server]] | Very Broad  | Simulates real-world usage. Tests a complete feature workflow, often involving the entire running application.                | External tools (Selenium, Postman), or full-stack `@SpringBootTest`<br>                                                        |                                                                        |
