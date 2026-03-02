
>[!Tdd]
>Test-Driven Development (TDD) is a software development methodology where **test code is written first**, **followed by the actual implementation code that satisfies those tests**.

- Usually used when
	- Developing new features
	- Improving legacy code
# Process
- diagram
	![[tdd.png|500]]
1. **Understand Requirements:** Clearly comprehend the feature request or bug report.
2. **Write Failing Test (Red):** Based on the understanding, write a unit test. Since no implementation code exists yet, this test will initially fail. _(Note: If hot-reloading is configured, tests may run automatically here.)_
3. **Implement Minimum Code (Green):** Write just enough production code to make the failing test pass. Run all tests; if all pass, proceed. If not, repeat this step.
4. **Refactor (Clean Code):** Once functionality is confirmed, refactor and improve the code (e.g., remove duplication, improve naming, optimize structure). All tests must still pass.
5. **Repeat:** Go back to step 1 for the next feature or bug, and **repeat (Rinse, lather, and repeat)** the cycle.
# Advantages of TDD
- Significantly improves **code quality**.    
- Encourages **feature decomposition** into small units, enabling a **divide-and-conquer** approach for development.
	- TDD를 잘하면 정말 잘게잘게 작은 단위로 나울 수 있음
- Clear understanding and definition of requirements.
- Enables stable refactoring.
- Minimizes regression errors.
- Ensures high test coverage.
# Need to know BDD (Behavior-driven development)
- Uses the **Given-When-Then** structure to write tests.
- Helps in designing **clear, behavior-focused test cases**.
- Often used to complement TDD by **making tests readable and aligned with business behavior**.

# Disadvantages
- high learning curve lol (u need to be good at using tools like Mockito)
	- Can get **very deep and complex** in practice.
    - Applying it in real-world projects is hard unless **all team members have strong TDD knowledge**.
- 어느 시점에서 어떤 테스트 객체가 필요할지 미리 생각하고 코드 작성 > 경험 기반. 어려움!
# Spring
- **Spring Boot Support:**
    - Spring is designed with **testable code in mind** from the start.

    - Offers **built-in support for various testing libraries**, making TDD easier to apply.
- [[Spring's main philosophies#POJO Based Development|Spring's POJO-based design]]

