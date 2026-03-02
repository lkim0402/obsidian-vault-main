
>[!Goal]
>The main purpose of initializing test data, as your notes point out, is to make your tests **deterministic** and **independent**.
>- **Deterministic**: A test should produce the same result every single time it's run.
>- **Independent**: The outcome of one test should never be affected by a test that ran before it.

# Basic setup with `@Sql`
- **Typical Use Case**: [[Integration Test|Integration tests]] or [[Functional Test|functional tests]] where you want precise control over the DB state via SQL
- **Scope**: Usually used in *service-layer* or *higher integration tests*, often with `@SpringBootTest` which loads the full app context.
	- [[Spring test module - spring-boot-starter-test#Key Testing Annotations|spring-boot-starter-test: Key Testing Annotations]]
- `Sql` is used to execute SQL scripts against a database for testing
	- can run before or after test methods
	- The simplest way to use `@Sql` is to point it at a script file, and this script will run _before_ each test method in the class. (Example 1 below)
	- A common pattern is to wipe the slate clean and then insert the specific data your test needs.
- There are other methods to initialize test data
	- `TestExecutionListener` - more advanced/powerful option
	- `@BeforeEach` - [[☕Java|java]] logic required


#### Example SQL test data (`test-data.sql`)
```sql
DELETE FROM products;
INSERT INTO products (id, name, price) VALUES (1, 'Product 1', 100.0);
INSERT INTO products (id, name, price) VALUES (2, 'Product 2', 200.0);
```
#### Example 1: Test class (`ProductServiceTest.java`)
- (Gemini: This code snippet is most likely an [[Integration Test]] or a [[Functional Test]]
```java
@SpringBootTest
// Before running tests in this class, execute the following SQL script.
@Sql("/test-data.sql")
class ProductServiceTest {
    // Your test code that assumes the products from the script exist.
}
```
- `@SpringBootTest` - Loads the full Spring Boot context.
	- [[Spring test module - spring-boot-starter-test#Key Testing Annotations|spring-boot-starter-test: Key Testing Annotations]]
#### Example 2: Advanced Usage with Options
```java
/**
 * Integration test class
 * - @SpringBootTest - Loads the full Spring Boot context.
 * - @Sql: Runs SQL scripts before/after tests to manage DB state.
 */
@SpringBootTest

/**
 * SQL scripts to run **before** each test method
 * - scripts: List of SQL files to execute (e.g., "/schema.sql", "/test-data.sql")
 * - executionPhase: BEFORE_TEST_METHOD means run before each test method
 * - config:
 *    - transactionMode = ISOLATED: Runs in a separate transaction from the test method,
 *      so effects remain even if the test rolls back.
 */
@Sql(
    scripts = {"/schema.sql", "/test-data.sql"},
    executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD,
    config = @SqlConfig(transactionMode = SqlConfig.TransactionMode.ISOLATED)
)

/**
 * SQL script to run **after** each test method
 * - "/cleanup.sql": Cleans up DB to prevent side effects on other tests
 * - executionPhase: AFTER_TEST_METHOD means run after each test method
 */
@Sql(
    scripts = "/cleanup.sql",
    executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD
)
class AdvancedSqlTest {
    // test methods here
}
```

- `executionPhase` can be set to `BEFORE_TEST_METHOD` or `AFTER_TEST_METHOD`.
- `@SqlConfig` allows advanced settings like transaction isolation.
	- `config = @SqlConfig(transactionMode = SqlConfig.TransactionMode.ISOLATED)`
		- This is *important*
		- Runs in a separate transaction from the test method,  so effects remain even if the test rolls back.