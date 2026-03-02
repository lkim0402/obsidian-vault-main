
>[!Overview]
>Data Access Layer (DAL) testing targets code that interacts directly with the database to verify **correctness**, **integrity**, and **reliability**.
>- [[⭐Layered Architecture#`@Repository`|Layered Architecture - Repository layer]] 
>- [[Spring Data JPA]]

- Beyond basic CRUD operations, this testing focuses on:
	- Is data accurately stored in the database?
	- Are redundant or unnecessary data prevented from being saved?
	- Do database constraints (e.g., UNIQUE, NOT NULL) function correctly?
	- Do custom queries return the expected results precisely?
- Tips
	- Always include **basic CRUD tests**, ensuring flows like create → save → retrieve → verify are covered.
	- Use H2 as a test-only database but configure constraints to mimic the production environment as closely as possible.
	- Include exception tests (e.g., `shouldThrow`) to improve test reliability.
	- For integration tests, use `@Transactional` with `@Rollback` to restore the database state after each test.
	- Use `@DataJpaTest` for slice testing at the repository layer; for full integration tests, configure additional setup.
# Purpose
## Verifying Data Persistence
- **Persistence** refers to the property of storing data outside the application memory
	- The key of this test is to verify that an entity object, once saved to the database, maintains the same values when retrieved again.
```java
@DisplayName("Verify data is persistently saved in DB upon product save")
class SaveProductTest {

    @Test
    @DisplayName("Saving a valid product persists it in the DB")
    void should_saveProduct_when_validInput() {
        // given: create product to save
        Product product = new Product("Mechanical Keyboard", 99000);

        // when: save product
        Product saved = productRepository.save(product);

        // then: verify saved product is not null and fields match
        assertThat(saved).isNotNull();
        assertThat(saved.getId()).isNotNull();
        assertThat(saved.getName()).isEqualTo("Mechanical Keyboard");
        assertThat(saved.getPrice()).isEqualTo(99000);
    }
}
```

Negative test case
```java
@DisplayName("Product save failure cases")
class SaveProductFailTest {

    @Test
    @DisplayName("Saving a product with negative price is rejected")
    void should_throwException_when_priceIsNegative() {
        // given: invalid product with negative price
        Product invalidProduct = new Product("Faulty Item", -5000);

        // when & then: expect exception on save
        assertThatThrownBy(() -> productRepository.save(invalidProduct))
            .isInstanceOf(DataIntegrityViolationException.class);
    }
}
```
- [[Exception handling#Exception Abstraction - `DataAccessException`|Exception Abstraction - DataAccessException]]
## Ensuring Data Consistency
- Consistency = *logical correctness of data*
- Ex)  two users cannot have the same email 
```java
@DisplayName("Exception occurs when saving duplicate emails")
class DuplicateEmailTest {

    @Test
    @DisplayName("Saving two users with the same email throws exception due to UNIQUE constraint")
    void should_fail_when_duplicateEmail() {
        // given: two users with the same email
        User user1 = new User("test@example.com", "Alice");
        User user2 = new User("test@example.com", "Bob");

        // when: save first user
        userRepository.save(user1);

        // then: saving second user throws exception
        assertThatThrownBy(() -> userRepository.save(user2))
            .isInstanceOf(DataIntegrityViolationException.class);
    }
}
```
## Database Constraint validation
```java
@DisplayName("NOT NULL Constraint Test")
@Nested
class NotNullTest {

    @Test
    @DisplayName("Saving fails when product name is null")
    void should_fail_when_nameIsNull() {
        // given: product created without a name
        Product product = new Product(null, 25000);

        // when & then: saving throws exception
        assertThatThrownBy(() -> productRepository.save(product))
            .isInstanceOf(DataIntegrityViolationException.class);
    }
}
```

| Constraint  | Description                                          | Exception Type                    |
| ----------- | ---------------------------------------------------- | --------------------------------- |
| NOT NULL    | Does not allow null values                           | `DataIntegrityViolationException` |
| UNIQUE      | Does not allow duplicate values                      | `DataIntegrityViolationException` |
| FOREIGN KEY | Ensures referential integrity (parent ID must exist) | `Constra`                         |
- [[Exception handling#Exception Abstraction - `DataAccessException`|Exception Abstraction - DataAccessException]]
## Verifying Query Result Accuracy
- Query result verification ensures that queries written by developers—such as **JPQL, SQL, Native Query, QueryDSL**—return the exact expected data.
	- [[Query Auto Generation]]
```java
// ProductRepository.java
public interface ProductRepository extends JpaRepository<Product, Long> {

    /**
     * Finds products whose names contain the given keyword.
     * @param keyword the search keyword
     * @return list of products with names containing the keyword
     */
    List<Product> findByNameContaining(String keyword);
}
```

```java
@DisplayName("Search products containing a specific keyword in the name")
@Nested
class FindByKeywordTest {

    @Test
    @DisplayName("Returns 2 products when searching for 'Mouse' after saving 2 such products")
    void should_returnProductsContainingKeyword() {
        // given: save 2 products containing 'Mouse'
        productRepository.save(new Product("Bluetooth Mouse", 25000));
        productRepository.save(new Product("Wired Mouse", 18000));
        productRepository.save(new Product("Keyboard", 30000));

        // when: search products with 'Mouse' in their name
        List<Product> result = productRepository.findByNameContaining("Mouse");

        // then: verify number of results and product names
        assertThat(result).hasSize(2)
                          .extracting(Product::getName)
                          .containsExactlyInAnyOrder("Bluetooth Mouse", "Wired Mouse");
    }
}
```
# What should be tested?

| Test Item                                                                          | Testing Needed?      | Explanation                                                |
| ---------------------------------------------------------------------------------- | -------------------- | ---------------------------------------------------------- |
| Queries using `@Query`<br>- [[Query Auto Generation]]                              | Required ✅           | Manually written queries are prone to typos or join errors |
| Method name queries with 2+ conditions                                             | Required ✅           | Auto-generated query logic may differ from intended logic  |
| Simple method name queries (e.g., `findById`)<br>- Built in by [[Spring Data JPA]] | Not needed ❌         | Well-tested built-in functionality in [[Spring Data JPA]]  |
| Single-condition field search (e.g., `findByEmail`)                                | Usually not needed ❌ | Exceptions allowed if field is critical to business logic  |
# `@DataJpaTest`
>[!Overview]
>`@DataJpaTest` is a specialized slice test annotation in Spring Boot designed for testing the JPA Repository layer.
>- [[Spring test module - spring-boot-starter-test#Key Testing Annotations|spring-boot-starter-test: Key Testing Annotations]]

- loads only the JPA-related beans selectively **without loading the entire application context**, enabling a **fast and lightweight testing environment**
- When to Use?
	- To verify that the repository’s basic CRUD operations work correctly
	- To validate the behavior and return values of custom query methods
	- When you want to reset the database state after tests
	- To quickly test JPA-related functionality without starting the full server
- Add `@Transactional` on top of the testing class (ex. `UserRepositoryTest.java`)
## Automatically loaded components
`@DataJpaTest` automatically loads the following components:

| Component                    | Role in `@DataJpaTest`                                                                                                                                                                                                  |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`@Entity` classes**        | Scans for all your entity classes (e.g., `Product`, `User`) so JPA knows about them.                                                                                                                                    |
| **`@Repository` interfaces** | Finds all your Spring Data JPA repositories (e.g., `ProductRepository`) and creates real instances of them.                                                                                                             |
| **`DataSource`**             | Configures a connection to a database. By default, it replaces your real database with a fast, in-memory H2 database.                                                                                                   |
| **`TestEntityManager`**      | **This is a key testing tool.** It's a simplified version of `EntityManager` with extra helper methods designed specifically for setting up data and making assertions in tests.<br>- [[JPA (Jakarta Persistence API)]] |
| **Transaction Management**   | Wraps each test in a transaction that is **rolled back** at the end, so tests don't interfere with each other.                                                                                                          |
- [[Spring Data Access Technologies]]
## Key Features Summary

| Feature                       | Description                                                                                                       |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `@DataJpaTest`                | Loads only JPA-related beans, resulting in faster tests                                                           |
| [[Slice Test\|Slice Testing]] | Tests specific layers without loading the full context                                                            |
| Built-in `@Transactional`     | Automatically rolls back transactions after each test method, resetting DB state<br>- [[Transaction]]             |
| Default DB: H2                | Uses in-memory H2 DB unless otherwise configured                                                                  |
| `@AutoConfigureTestDatabase`  | Can be used to replace the default DB with an actual database                                                     |
| TestEntityManager Support     | Provides a test-friendly helper that extends [[JPA (Jakarta Persistence API)#EntityManager\|JPA’s EntityManager]] |
## Test Data Preparation Strategies
- They are *different* from [[Initializing test data with @Sql]] (used for service or above, and for [[Integration Test|integration tests]], [[Functional Test|functional tests]])
- For us rn, these strategies apply to [[Unit Test|unit tests]] or [[Slice Test|slice tests]] on the [[⭐Layered Architecture#`@Repository`|repository layer]]
- Tips
	- **Simple CRUD tests**: Quickly write setup using `@BeforeEach`
	- **Complex domain objects**: Recommend using **Fixture**, **Factory**, or **Builder pattern**
	- When separating **unit tests and integration tests**, it’s also good practice to manage **test-specific preprocessing utility classes separately**

|Strategy|Description|Recommended Usage|
|---|---|---|
|`@BeforeEach`|Directly create common data before each test|Simple test environment setup|
|Fixture Class|Separate reusable object creation methods|Repeated object setups across multiple tests|
|Fixture + Builder|Achieve flexibility and reusability simultaneously|When various field combinations are needed|
#### [[JUnit 5#`@BeforeEach` / `@AfterEach` + `@BeforeAll`/ `@AfterAll`|Using JUnit5's @BeforeEach]]
```java
@BeforeEach
void setUp() {
    memberRepository.deleteAll();  // Remove previous test data
    memberRepository.save(new Member("hgd@gmail.com", "Hong Gil-dong", "010-1111-2222")); // Set common test data
}
```
- When only simple common data is needed	
- Quick to set up, but may cause code duplication

#### Reusable fixtures with fixture classes
```java
public class MemberFixture {
    public static Member createHgd() {
        return new Member("hgd@gmail.com", "Hong Gil-dong", "010-1111-2222");
    }

    public static Member createLee() {
        return new Member("lee@gmail.com", "Yi Sun-shin", "010-3333-4444");
    }
}
```
- A **fixture** is a helper class that assists in creating test objects.
- Separating methods with meaningful names improves readability and helps **eliminate duplication in tests**.
- Use when repeatedly creating objects with the same structure across multiple tests	
#### Builder Pattern + fixture
```java
public class MemberFixture {
    public static MemberBuilder builder() {
        return new MemberBuilder()
                .email("test@example.com")
                .name("Tester")
                .phone("010-0000-0000");
    }
}

memberRepository.save(
    MemberFixture.builder().name("홍길동").email("hong@example.com").build()
);
```

## Verification Methods
- In JPA Repository testing, verification methods mainly fall into two categories:
	1. **Return Value (State) Verification**: Confirm that a specific query method behaves as expected
	2. **Database State Verification**: Confirm that the number of stored entities and their field values are correctly persisted

| Strategy              | Description                                                                                         | Example Usage                       |
| --------------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------- |
| State Verification    | Check if return values from `findByXxx()` match expectations<br>- single-entity lookup              | `assertThat(optional).isPresent()`  |
| DB State Verification | Check if saved count and field values are correctly reflected in DB<br>- multiple entity properties | `hasSize(n)`, `extracting("email")` |
#### Return value
```java
@Test
void findByEmail_shouldReturnMemberIfExists() {
    // given: prepare and save a member entity
    Member member = new Member("hgd@gmail.com", "Hong Gil-dong", "010-1111-2222");
    Member savedMember = memberRepository.save(member);

    // when: find member by email
    Optional<Member> found = memberRepository.findByEmail("hgd@gmail.com");

    // then: verify presence and correct email
    assertThat(found).isPresent();  // Optional is not empty
    assertThat(found.get().getEmail()).isEqualTo("hgd@gmail.com");
}
```
- Suitable for validating the **correct operation of *single-entity lookup* methods** like `findByEmail()`.
- The main goal is to **test the behavior of JPA query methods**, hence fits [[Slice Test|slice testing]].
#### Database State
```java
@Test
void findAll_shouldReturnAllSavedMembers() {
    // given: save two members
    memberRepository.save(new Member("kim@test.com", "Kim Cheol-soo", "010-2222-3333"));
    memberRepository.save(new Member("lee@test.com", "Lee Young-hee", "010-4444-5555"));

    // when: retrieve all members
    List<Member> members = memberRepository.findAll();

    // then: verify the count and email values
    assertThat(members).hasSize(2);
    assertThat(members)
        .extracting("email")
        .containsExactlyInAnyOrder("kim@test.com", "lee@test.com");
}
```
- Checks if the **number of stored entities and their field values match expectations** via `findAll()`.
- `extracting()` extracts specific fields from a list of entities, making it very useful for validating ***multiple entity properties***.
## Other environments

| Annotation      | Target Technology         | Main Components Loaded                                 | Focus                                                                       |
| --------------- | ------------------------- | ------------------------------------------------------ | --------------------------------------------------------------------------- |
| `@DataJpaTest`  | Spring Data JPA (JPA ORM) | JPA entities, repositories, EntityManager, Hibernate   | Full JPA persistence layer testing (entity mapping, JPQL, caching, proxies) |
| `@JdbcTest`     | Plain Spring JDBC         | JdbcTemplate, DataSource, TransactionManager           | Raw JDBC query and database interaction testing                             |
| `@DataJdbcTest` | Spring Data JDBC          | Spring Data JDBC repos, DataSource, TransactionManager | Testing simplified JDBC-based repositories (no ORM)                         |

## Add `EnableJpaAuditing`
You have to put `@EnableJpaAuditing` on your test class because `@DataJpaTest` does **not** automatically enable JPA Auditing.
 - `@DataJpaTest` is a "slice" test annotation. It's designed to load only the necessary components for testing JPA repositories, which includes:
	- The in-memory database configuration
	- [[Spring Data JPA]] repositories.
	- JPA entities and entity manager.
- It specifically **excludes** components that are not directly related to the JPA layer. JPA Auditing, which automatically populates fields like `createdDate` or `lastModifiedDate`, is a separate feature provided by Spring Data. To make it work, you need to explicitly enable it.
- Since your `application-test.yml` file is for configuring things like the database URL and logging levels, it cannot enable a Spring feature like JPA Auditing, which requires a specific annotation on a configuration class or a test class.
- Adding `@EnableJpaAuditing` to your `@DataJpaTest` class or a test configuration class is the correct way to ensure that your auditable fields are populated during repository tests.

```java
  
@EnableJpaAuditing  
@DataJpaTest  
public class ChannelRepositoryTest {
	...
}
```