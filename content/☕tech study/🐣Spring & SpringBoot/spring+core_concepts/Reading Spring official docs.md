# Documentation Structure
For learning and practical application of Spring, the three essential types of documents are:
- **Reference Documentation**
- **API Documentation**
- **Guides**

>[!summary]
>- **Reference:** For understanding features/configurations.
>- **API Docs:** For class/annotation definitions.
>- **Spring Guides:** For quick hands-on learning.
## Reference Documentation
https://docs.spring.io/spring-framework/reference/index.html
- Spring's official feature manual, the **most comprehensive technical document**. Covers framework philosophy, feature explanations, configuration, and examples
- **style**
	- narrative explanations + code examples
	- Organized by module for easy navigation
	- Separate documentation exists for each Spring version (e.g., v5.3, v6.0)
- **Tip**
	- Use when first encountering a concept / when wanting deep understanding of operational principles
	- Focus on *understanding framework mechanics and structure* rather than just usage
## API Documentation (Javadoc)
https://docs.spring.io/spring-framework/docs/current/javadoc-api/
- **Overview** 
	- Detailed documentation for classes, interfaces, and methods within Spring's source code. 
	- Formatted as *Javadoc*.
- **Characteristic**
	- Allows readers to *quickly grasp definitions, signatures, arguments, exceptions, and inheritance structures of specific classes or methods*
	- Automatically generated, useful for checking the latest code structure.
- **Tip** 
	- Use to *verify official method definitions directly* (rather than relying on IDE autocompletion)
	- Useful for checking core types like `BeanDefinition`, `ApplicationContext`, `@Transactional`.
## Guides
https://spring.io/guides
- **Overview** 
	- A series of *practical, learning-oriented guides* provided directly by the Spring team
	- "How to implement X?" + code examples
- **Characteristics** 
	- *Example/Code-focused, step-by-step tutorials* (suitable for quick hands-on practice)
	- Covers practical topics like "Building a Login Feature with Spring Security," "Saving Data with JPA."
- **Tip**
	- Useful when *encountering new technologies* (e.g., Spring Data JPA, OAuth2) for the first time, or when you *prefer to "learn by building"* rather than just theory.
# Effective Documentation Search and Utilization
>Spring's documentation is vast and multi-layered, requiring "search" and "utilization strategies" rather than just "reading." 

## Keyword-Based Search Strategy
- Google
	- Combine **specific features + annotations + library names**
	- Append `site:docs.spring.io` to your Google search to restrict results to official documentation
	- Examples
		- For `@Transactional` in Spring Boot: `spring boot @Transactional site:docs.spring.io`
		- Dependency Injection: `spring constructor injection site:docs.spring.io`
        - Bean Scope: `spring bean scope site:docs.spring.io`
        - Transaction Propagation: `@Transactional propagation site:docs.spring.io`
## Utilizing Example Code
- **Usage Tip**
	- Reference Documentation: Code examples focus on operational principles, not always direct practical scenarios.
    - Spring Guides: Provide quick-to-follow tutorial-style code.
    - Recommended: Download and analyze full example projects from the official GitHub repository ([https://github.com/spring-projects](https://github.com/spring-projects))
- **Learning Approach Tip** 
	- Understanding and tracing code flow in Spring documentation provides significant learning benefits. 
	- Don't try to memorize API usage -> become familiar with the structure so you can always look things up. 
	- Remembering _where_ certain information is, so you can find it when needed, is far more efficient.
## Referring to Version-Specific Documentation
- **Version Check Tip** 
	- Spring has distinct changes between versions, so always refer to the documentation matching your project's version. 
	- Check your project's `spring-boot-starter-parent` or `spring-framework` version.
- **Troubleshooting** 
	- If errors occur due to version differences, finding the correct example in that specific version's documentation is crucial.

