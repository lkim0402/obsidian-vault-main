
>[!Overview]
>Unit testing means "**testing small units of functionality or methods to verify they produce the expected results.**"

![[spring-testing.png|900]]
- A **unit test** 
	- about testing a specific, isolated part of the application. 
	- For example, in our coffee order sample app, the “unit” could be the service layer or utility classes used by the service layer
		- The service layer contains the core **business logic**, which is usually the best target for unit testing *because it can be tested independently*.  
		- [[⭐Layered Architecture#`@Service`|Layered Architecture: Service layer]]
	- In practice, most unit tests are written **at the method level**.
- A **test case**
	- A specification that defines the input data, execution conditions, and expected results for testing a unit such as a method. In short, it’s the test code you write to verify a single unit, containing the logic for inputs, conditions, and expected outcomes.
- Tests involving a [[🗃️Database|database]]
	- If a test interacts with a database but leaves the database state unchanged before and after the test, it can still be considered a unit test
	- However, unit tests are ideally as independent and as small in scope as possible
# Guidelines: F.I.R.S.T Principles
1. **Fast**: Tests must run quickly to encourage frequent execution and early problem detection.
2. **Independent**: Tests should run without influence from others; execution order must not affect results.
3. **Repeatable**: Tests yield consistent results regardless of environment; *avoid external dependencies*.
4. **Self-validating**: Tests automatically assert pass/fail without manual inspection.
	- You shouldn’t have to inspect logs or manual outputs—the test itself should assert correctness automatically
5. **Timely**: Tests are ideally written *before or alongside feature implementation* to ensure alignment with intended behavior.
	- Even if you implement first, avoid writing tests long after the code is done—incrementally adding tests as you develop each part is more efficient
# Example
```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}

class CalculatorTest {

    @Test
    void testAdd() {
        Calculator calculator = new Calculator();
        int num1 = 3;
        int num2 = 5;
        int result = calculator.add(num1, num2);
        assertEquals(num1 + num2, result);  // 기대값과 실제값 비교
    }
}
```