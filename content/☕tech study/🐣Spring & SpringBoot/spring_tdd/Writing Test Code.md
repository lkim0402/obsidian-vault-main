- [[Spring test module - spring-boot-starter-test#Key Testing Annotations| spring-boot-starter-test: Key Testing Annotations]]
- [[JUnit 5]]
- Test classes should be placed under the `src/test/java` directory so that testing tools can automatically detect and run them.
# Isolating tests
```java
@SpringBootTest
@Transactional
class TransactionalTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    void createUser() {
        User user = new User("testuser", "test@example.com");
        userRepository.save(user);
        // Automatically rolled back after test completes
    }

    @Test
    void getUser() {
        // given
        User user = new User("testuser", "test@example.com");
        User savedUser = userRepository.save(user);

        // when
        Optional<User> findUser = userRepository.findById(savedUser.getId());

        // then
        // findUser.get().getUsername().equals(savedUser.getUsername()) -> true expected
        // but result might be false if not handled properly
    }
}
```
- Each method annotated with `@Test` is run individually and independently by the test framework
	- each method runs inside its own [[Transaction]], which is automatically rolled back after the method finishes
	- Changes made in one test method do **not** affect other test methods.
	- The database state is reset after each test method, keeping tests isolated.
- Adding `@Commit` commits the [[Transaction]] instead of rolling back.
	- Transaction behavior can vary based on propagation level.
- When used with [[Spring Data Access Technologies#Representative Technologies JPA + Spring Data JPA|JPA]], pay attention to flush timing and persistence context management.
- `@SpringBootTest`
	- loads the entire application context, which can be slow.
	- Use it only when necessary, and prefer slice tests like `@WebMvcTest` or `@DataJpaTest` for faster, more focused testing.
