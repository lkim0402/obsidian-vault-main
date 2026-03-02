# Spring REST Docs
>[!Definition]
>**Philosophy: "Test-Driven Documentation"**
>- This tool generates documentation from the output of your **unit and integration tests**. The core idea is that if your tests pass, your documentation is guaranteed to be accurate.

- Not that used as much compared to Swagger, but [[🐣Spring & SpringBoot|Spring]] supports it sm so it's not bad to learn it
- **How It Works**
    1. You write detailed tests for your API controllers (often **slice tests** that focus on the web layer).
    2. *You use libraries like **Mockito*** to mock dependencies and control test conditions.
    3. When the tests run successfully, REST Docs *generates small documentation* files called **snippets** (in `.adoc` format).
    4. You then assemble these snippets into a complete API document.
- **Pros**
    - ✅ Guaranteed Accuracy:  The documentation is always synchronized with your code's actual behavior because it's generated from passing tests.
- **Cons**
    - ❌ Mandatory Testing: You **MUST** write a test for every endpoint and feature you want to document. This can be time-consuming...
	    - [[Test-Driven Development (TDD)|TDD]]는 기본임, 기본적으로 테스트 코드 짜는 것에 능숙해야함 (high learning curve)
	    - **Slice test** => To generate documentation for an API endpoint, you need to test the Controller layer
		    - load only the web layer (controller + [[⭐Spring MVC]]), and *mock the other layers*
    - ❌ High Learning Curve: It requires a strong understanding of testing methodologies like TDD, as well as proficiency with testing libraries (JUnit, Mockito). It can be difficult for beginners.
# Swagger UI
>[!Definition]
>**Philosophy: "Annotation-Based Documentation"**
>- Generates an interactive API console in your browser based on annotations you add directly to your [[⭐Layered Architecture#`@Controller` / `@RestController`|Controller]] and [[Data Transfer Object (DTO)|DTO]] code.
>- More common , better for rapid development, easy to get started


- **How It Works**
    1. Add the `springdoc-openapi` dependency to your `build.gradle` or `pom.xml`.
    2. Add annotations like **`@Operation`**, **`@Tag`**, and **`@Schema`** to your controller methods and DTO classes.
    3. Run your application and navigate to `/swagger-ui.html`.
- **Pros**
    - ✅ Easy to Start: It's very fast to set up. You can generate basic documentation without writing any tests.
    - ✅ Interactive UI: It provides a user-friendly interface where developers can see and **test your API endpoints directly in the browser**.
    - ✅ Industry Standard: This is the most popular and widely used tool for API documentation.
- **Cons**
    - ❌ Potential for Inaccuracy: Because the documentation isn't tied to tests, it's possible for the annotations (especially descriptions and examples) to become outdated or different from the actual API logic if not carefully maintained.
- **Others**
	- `GroupedOpenApi` is a feature used to organize API documentation into different groups
		- useful in [[Microservice Architecture (MSA)]]
	- NO NEED to write separate test files, just add annotations
		- Use Swagger UI's "`Try it out`" button 
	- If you build an automated deployment pipeline, the **documentation can be updated automatically**
		- CI/CD - Because the docs are part of the code, you can automate updates
		- `Code update -> GitHub -> Docs update`
- links
	- [official github docs.. ](https://github.com/swagger-api/swagger-core/wiki/Swagger-2.X---Annotations)
	- [티스토리 블로그](https://leejunkim.tistory.com/32)