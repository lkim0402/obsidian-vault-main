
>[!Overview]
>Spring Boot offers strong built-in support for testing. The `spring-boot-starter-test` starter includes various testing features, helping developers write tests without complicated setup.

# `spring-boot-starter-test`
- Spring Boot provides the `spring-boot-starter-test` starter for [[Integration Test|integration testing]]
- automatically includes key libraries such as [[JUnit 5]], [[Mockito]], AssertJ, Hamcrest, and Spring Test

# Main libraries included:

| Library          | Description                                                       | Notes                                                                                                                                                                                                                 |
| ---------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[JUnit 5]]      | The leading Java testing framework                                | Using version 5 (which is different from 4). Version 3 is not used much.<br>- Ver 5 is the standard<br>- Explicitly state diff. version if you need to                                                                |
| [[Mockito]]      | Creates mock objects                                              | Used to *disconnect (단절)* layers for [[Unit Test\|unit testing]], [[Slice Test\|slice testing]]                                                                                                                       |
| AssertJ          | A library used for validation & gives assertion syntax. <br>      | - Provides readable, fluent assertion syntax<br>- Used together with JUnit. Friendly to developers                                                                                                                    |
| Hamcrest         | Matcher-based assertion tool                                      | - **alternative** to AssertJ for assertions<br>- style is closer to english<br>- what to choose is just team/personal preference                                                                                      |
| Spring Test      | Supports Spring ApplicationContext and MockMvc                    | Enables dependency injection (`@Autowired`) in tests by loading the Spring container (`ApplicationContext`)<br>- [[IoC (Inversion of Control)#`ApplicationContext`\|IoC (Inversion of Control) - ApplicationContext]] |
| JSONassert       | Useful for comparing JSON responses                               | Intelligently compares two JSON strings, ignoring formatting and field order, to check if they are semantically the same                                                                                              |
| Spring Boot Test | Supports [[Integration Test\|integration testing]] in Spring Boot | - Simplifies integration testing with `@SpringBootTest`. <br>- Provides "slice test" annotations (e.g., `@WebMvcTest`, `@DataJpaTest`) to test specific application layers quickly.                                   |
# Key Testing Annotations
>Basically ALL OF THESE ARE IMPORTANT AND YOU NEED TO KNOW EVERYTHING

Spring provides various annotations to simplify writing tests.
Key annotations include:

| Annotation        | Library          | Description                                                                                 | Common Test Type(s)                                                |
| ----------------- | ---------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `@SpringBootTest` | Spring Boot Test | Loads the entire application context for testing                                            | [[Integration Test\|Integration]], [[Functional Test\|Functional]] |
| `@WebMvcTest`     | Spring Boot Test | Loads only the web layer (e.g., `Controller`) for slice testing                             | [[Slice Test\|Slice]] (Web Layer)                                  |
| `@DataJpaTest`    | Spring Boot Test | Loads only the persistence layer (JPA) for slice testing<br>- [[Data Access (Repository) Layer Testing]] | [[Slice Test\|Slice]] (Data Layer)                                 |
| `@MockBean`       | Spring Boot Test | Replaces a Spring bean with a **[[Mockito]]** mock in the context                           | [[Slice Test\|Slice]],  [[Integration Test\|Integration]]          |
| `@SpyBean`        | Spring Boot Test | Wraps a real Spring bean with a **[[Mockito]]** spy in the context                          | [[Slice Test\|Slice]],  [[Integration Test\|Integration]]          |
| `@Test`           | [[JUnit 5]]      | Marks a method as a test method that should be run                                          | **All** (Unit, Slice, etc.)                                        |
| `@BeforeEach`     | [[JUnit 5]]      | Runs a setup method before _each_ `@Test` method                                            | **All** (Unit, Slice, etc.)                                        |
| `@AfterEach`      | [[JUnit 5]]      | Runs a teardown method after _each_ `@Test` method                                          | **All** (Unit, Slice, etc.)                                        |
- `@SpringBootTest`
	- loads the entire application context, which can be slow.
	- Use it only when necessary, and prefer slice tests like `@WebMvcTest` or `@DataJpaTest` for faster, more focused testing.
- [[JUnit 5]] -> explains more about the key annotations
# Example Code
```java
@SpringBootTest
class OrderServiceTest {

    @Autowired
    private OrderService orderService;

    @Test
    void createOrder_ShouldSucceed() {
        // given
        OrderRequest request = new OrderRequest("Americano", 2);

        // when
        Order order = orderService.createOrder(request);

        // then
        assertThat(order.getQuantity()).isEqualTo(2);
    }
}
```

