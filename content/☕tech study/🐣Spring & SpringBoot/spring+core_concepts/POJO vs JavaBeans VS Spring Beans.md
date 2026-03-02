# POJO (Plain Old Java Object)
> A pure Java object that doesn't depend on a specific framework/ container
- Focuses on business logic without framework ties
- Highly testable and reusable
- Characteristics
	- No special [[Abstraction - Classes & Interfaces#Interface|interfaces]] or [[Inheritance in java|inheritance]] beyond [[☕Java]] standards.
    - Works without [[Java Annotation|annotations]] or configuration files.
    - Typically just has getters/setters and a default [[Constructor & this|constructor]].
# JavaBeans
> A specific type of POJO that follows certain conventions (rules) for its properties and behavior.
- Private fields with public getter/setter methods (`getProperty()/setProperty()`).
- A public no-argument constructor.
- May implement `java.io.Serializable`.
- **Purpose:** Enable tools to inspect and manipulate component properties automatically, often used for GUI components or data transfer objects.
# Bean
- An object managed by the Spring IoC container.
- [[Bean]]