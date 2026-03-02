
>[!Overview]
>Bean Validation is a Java standard (JSR 380) that provides a *consistent way to perform validation on Java [[Bean]] objects*.
>- primarily uses annotations to define constraints and supports validation at the field, method parameter, constructor parameter, and class levels

- Spring fully integrates `Bean Validation` and offers the following features:
	- Automatic validation of controller method parameters
	- Declarative validation support via `@Valid` and `@Validated` annotations
	- Exception handling mechanisms for validation failures
	- Support for custom validator registration and extension
- Bean VS Bean validation
	- [[Bean]] - spring container가 관리하는 객체
	- [[POJO vs JavaBeans VS Spring Beans#JavaBeans|JavaBeans]] 
		- The **Jakarta Bean Validation** specification (which gives us annotations like `@NotNull` and `@Valid`) was named this way because it was designed to validate the data held within these simple **JavaBeans**
- Tips
	- Validation annotations can be applied to fields, parameters, return values, and more.
	- The `@Validated` annotation is more flexible than `@Valid`, allowing the use of validation groups.
	- You can create custom annotations to abstract complex business rules into reusable validators.
	- In production, separate validation failure messages between logging and user feedback for better management.
- [[Input validation locations (service, controller)]]
- [[Custom Validator]]

`build.gradle`
```groovy
implementation 'org.springframework.boot:spring-boot-starter-validation'
```
- **transitively includes** these dependencies (or compatible versions):
	- `org.hibernate.validator:hibernate-validator`
	- `org.glassfish:jakarta.el`
# Why
- User may make mistake/intentionally submit bad data
- If unchecked this can led to these problems
	- **Compromised Database Integrity**: Storing incorrect or inconsistent data.
	- **System Exceptions**: Causing the application to crash or behave unpredictably.
	- **Business Logic Errors**: Leading to incorrect calculations or process flows.
	- **Security Threats**: Opening vulnerabilities like SQL Injection, Cross-Site Scripting (XSS), etc.
- [[4 Potential problems in a Spring application#Invalid User Input|4 Potential problems in a Spring application - Invalid User Input]]
# Main annotations
> NEED TO KNOW EVERYTHING LOL
- Examples here: https://calm-individual-12a.notion.site/246c6b70982881628d4ad40d15d3c2fd

| Category   | Annotations                                  | Description                            |
| ---------- | -------------------------------------------- | -------------------------------------- |
| String     | `@NotNull`, `@NotEmpty`, `@NotBlank`         | Validate null, empty, or blank strings |
| String     | `@Size`, `@Length`                           | Validate string length                 |
| String     | `@Email`, `@Pattern`                         | Format validation (email, regex)       |
| Number     | `@Min`, `@Max`, `@Range`                     | Numeric range constraints              |
| Number     | `@Positive`, `@Negative`                     | Sign validation (positive/negative)    |
| Number     | `@Digits`, `@DecimalMin`, `@DecimalMax`      | Precision and decimal validation       |
| Date       | `@Past`, `@Future`, etc.                     | Time-based validation                  |
| Collection | `@Size`, `@Valid`, element-level annotations | Validate lists/maps and their elements |
- `@Pattern`
	- Regex is still widely used
	-  even tho GPT is good at it, it will still be beneficial (and won't hurt you) if you get used to it 
#### `@NotNull, @NotEmpty, @NotBlank`

|Annotation|`null`|`""` (empty)|`" "` (whitespace)|Best For|
|---|---|---|---|---|
|**`@NotNull`**|❌|✅|✅|Any object type|
|**`@NotEmpty`**|❌|❌|✅|Collections, Strings|
|**`@NotBlank`**|❌|❌|❌|Strings only|

# Before and after bean validation
## Before (Not using)
```java
public class User {
    private String username;
    private String email;
    private int age;

    public User(String username, String email, int age) {
        this.username = username;
        this.email = email;
        this.age = age;
    }

    // 게터 생략
}
```

```java
public class Validator {
    public static void validateUser(User user) {
        if (user.getUsername() == null || user.getUsername().isBlank()) {
            throw new IllegalArgumentException("사용자 이름은 필수입니다.");
        }
        if (!user.getEmail().contains("@")) {
            throw new IllegalArgumentException("이메일 형식이 올바르지 않습니다.");
        }
        if (user.getAge() < 18) {
            throw new IllegalArgumentException("나이는 18세 이상이어야 합니다.");
        }
    }
}
```
## After (Using)
```java
import jakarta.validation.constraints.*;

public class User {

    @NotBlank(message = "사용자 이름은 필수입니다.")
    private String username;

    @Email(message = "이메일 형식이 올바르지 않습니다.")
    private String email;

    @Min(value = 18, message = "나이는 18세 이상이어야 합니다.")
    private int age;

    public User(String username, String email, int age) {
        this.username = username;
        this.email = email;
        this.age = age;
    }

    // 게터 생략
    public String getUsername() { return username; }
    public String getEmail() { return email; }
    public int getAge() { return age; }
}
```



# Validation in Client & Server side

Validation should happen in two key places, with each having a distinct role.
- [[Backend & Main Components Explained#Client-side vs Server-side|Backend Explained - Client-side vs Server-side]]

| Location                          | Primary Role                                                                                                                                                         | Examples                                                                                                                                                                                               |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Client-Side** (The Browser)     | Improve **User Experience (UX)** by providing instant feedback.                                                                                                      | Using HTML5 attributes like `required` or `type="email"`, and performing checks with JavaScript before submitting a form                                                                               |
| **Server-Side** (The Application) | Act as the **final authority** to guarantee data integrity and enforce security. **Client-side validation can be bypassed, so server-side validation is mandatory.** | •  **`@Valid`** and **`@Validated`** annotations <br>• Flexibly handling validation errors using the `BindingResult` object<br>• Validating complex business rules separately within the service layer |
# Some other resources
- 정규 표현식에 대해서 더 알아보고 싶다면 아래 링크를 확인하세요.
    정규 표현식은 프로그래밍 언어에 종속되지 않은 표현식입니다. 따라서 아래 관련 자료에서 Java와 관련된 자료가 아니어도 정규 표현식을 학습하는데 지장이 없으니 이 점 참고해서 관련 자료를 살펴보기 바랍니다.
    - 정규 표현식 관련 자료
        - [https://www.w3schools.com/java/java_regex.asp](https://www.w3schools.com/java/java_regex.asp)
        - [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions)
    - 정규 표현식 모범 사례 자료
        - [https://docs.microsoft.com/ko-kr/dotnet/standard/base-types/best-practices](https://docs.microsoft.com/ko-kr/dotnet/standard/base-types/best-practices)
- Jakarta Bean Validation에 대해서 더 알아보고 싶다면 아래 링크를 확인하세요.
    - Jakarta Bean Validation Specification
        - [https://beanvalidation.org/2.0/spec/](https://beanvalidation.org/2.0/spec/)
    - Jakarta Bean Validation Built-in Constraint definitions
        - [https://beanvalidation.org/2.0/spec/#builtinconstraints](https://beanvalidation.org/2.0/spec/#builtinconstraints)
    - Hibernate Validator
        - [https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#preface](https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#preface)
- Java Bean에 대해서 더 알아보고 싶다면 아래 링크를 확인하세요.
    - Java Bean 관련 자료
        - [](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88)[https://ko.wikipedia.org/wiki/자바빈즈](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88)
- 이메일 주소의 스펙에 대해서 더 알아보고 싶다면 아래 링크를 참고하세요.
    - [https://en.wikipedia.org/wiki/Email_address](https://en.wikipedia.org/wiki/Email_address)