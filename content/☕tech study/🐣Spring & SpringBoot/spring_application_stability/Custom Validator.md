
>[!Overview]
>You can create a **Custom Validator** to *implement your own validation logic* when the built-in annotations are not sufficient
>- [[Bean validation (Input validation)#Main annotations|Bean validation (Input validation) - Main annotations]]
>- If you use this well it can get veeeery convenient

- Example when you need to make one
	- When you need to validate that a string is not blank (beyond null checks)
	- When you need to verify that an enum value is included in a specific set of allowed values
	- When you need to compare the values of two different fields
- Convenient when dealing with `enums`!
# Steps
1. **Define a custom annotation** (`@interface`)
2. **Implement the `ConstraintValidator` interface** and write the validation logic
3. **Apply the custom annotation** to the relevant field(s) in your DTO class
# Example of making a custom validator
#### 1. Define a custom annotation (`@interface`)
- just metadata
- Includes
	- the default error message
	- optional `groups` and `payload` (Bean Validation requires these)
    - the link to the class that actually runs the check (`validatedBy = NotSpaceValidator.class`).
```java
// Uses Constraint annotation and Payload interface from javax.validation package
import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.*;

/**
 * Custom annotation to validate that a string is not composed solely of whitespace.
 * - Ensures the value is not null and does not consist only of whitespace characters (" ")
 * - The actual validation logic is implemented in NotSpaceValidator.class
 */
@Target(ElementType.FIELD) // This annotation can only be applied to fields
@Retention(RetentionPolicy.RUNTIME) // Must be retained at runtime so validation can occur
@Constraint(validatedBy = {NotSpaceValidator.class}) // Specifies the Validator class that performs the check
public @interface NotSpace {

    /**
     * Default message returned when validation fails
     * - Can be overridden using @NotSpace(message = "...")
     */
    String message() default "Must not be blank";

    /**
     * groups attribute
     * - Used to specify Bean Validation groups
     * - Rarely used in simple cases, but included for extensibility
     */
    Class<?>[] groups() default {};

    /**
     * payload attribute
     * - An extension point for carrying metadata
     * - Not commonly used, but useful when integrating with certain frameworks
     */
    Class<? extends Payload>[] payload() default {};
}
```
- **`@Target(ElementType.FIELD)`** 
	-  "Where can I put this sticker?"
	- `ElementType.FIELD` ->  you can only apply `@NotSpace` to class fields (variables). You couldn't put it on a whole class or a method.
- **`@Retention(RetentionPolicy.RUNTIME)`** 
	- This answers, "How long should this sticker last?" 
	- `RetentionPolicy.RUNTIME` -> the annotation's information must be available to the Java Virtual Machine (JVM) while the program is **running**.
	- This is essential because the validation framework needs to read the annotation at runtime to perform the check.
- `@Constraint(validatedBy = {NotSpaceValidator.class})`
	- This is the most important part. It's the link that connects your annotation sticker (`@NotSpace`) to the class that contains the actual validation logic (`NotSpaceValidator`).
	- It tells the validation framework, "When you see the `@NotSpace` sticker, go run the code inside the `NotSpaceValidator` class to check if the rule is broken."
- **`message()`, `groups()`, `payload()`** ->These are mandatory attributes required by the Bean Validation specification
    - `message()`: Defines the default error message to show if validation fails.    
    - `groups()`: This is an advanced feature for grouping validation rules. For example, you could have one group of rules for creating a user and another for updating them.         
    - `payload()`: An even more advanced feature for carrying extra metadata with your annotation. You also just leave it as a default.
#### 2. Implement the `ConstraintValidator` interface and write the validation logic
```java
import org.springframework.util.StringUtils;
import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

// we just made NotSpace!
public class NotSpaceValidator implements ConstraintValidator<NotSpace, String> {

    @Override
    public void initialize(NotSpace constraintAnnotation) {
        // Initialization logic (if needed)
    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        // Allow null, but reject strings that contain only whitespace
        return value == null || StringUtils.hasText(value);
    }
}
```
- ``public class NotSpaceValidator implements ConstraintValidator<NotSpace, String>``
	- Implement the `ConstraintValidator<A, T>` interface.
		- **A**: The custom annotation type (`@NotSpace`)
		- **T**: The type of the field being validated (`String`)
- Methods
	- **`initialize()`**
		- Called once before validation begins
		- purpose: read configuration from your annotation
		- For a simple annotation like `@NotSpace`, you don't need any configuration, so the method is empty.
		- However, imagine if your annotation was `@MaxWordCount(value = 10)`. You would use the `initialize()` method to get that `10` and save it so `isValid()` can use it later
	- **`isValid()`** – Returns `true` if:
		- Every time [[Bean validation (Input validation)|bean validation]] sees `@NotSpace` on a field, it calls `isValid(...)` to decide if the value is OK.
	- These methods can be *automatically created in Intellij* (empty methods)
#### 3. Applying to DTO
```java
public class MemberPatchDto {
    private long memberId;

    @NotSpace(message = "Member name must not be blank")
    private String name;

    @Pattern(
        regexp = "^010-\\d{3,4}-\\d{4}$",
        message = "Phone number must start with 010 and be in the format of 11 digits with '-'")
    private String phone;

    // Getters/Setters omitted
}
```
# Custom Validator VS Regex

|Item|Regular Expression-based Validation|Custom Validator-based Validation|
|---|---|---|
|Advantages|Concise and reusable|Supports complex logic, better readability|
|Disadvantages|Potential performance issues, complex regex|Requires implementation, somewhat verbose|
|Examples|Using `@Pattern`|`@NotSpace`, Enum validation, etc.|
- Avoid ***overly complex regex for validation*** -> *Catastrophic Backtracking*
	- regex engines often work by trying to match a pattern, and if they fail, they **backtrack** to a previous point and try a different path
	- **Catastrophic Backtracking** happens when a complex regex with nested, repetitive patterns (like `(a+)+`) tries to match a string that _almost_ fits but doesn't. The regex engine has to try an exponential number of paths before it can finally give up and say "no match." This can cause the CPU to spike to 100% and freeze your application for seconds or even minutes.
- When your validation logic becomes complex, a custom validation annotation is often a better choice
- When to use each
	- **Regex** - for simple, well-defined patterns (emails, zip codes, phone numbers).
	- **Custom validator**  - the logic is complex, involves multiple conditions, or requires high performance and reliability
# Tips
## Designing with Null Acceptance in Mind
- Most validators consider `null` as a valid value. 
- Because of this, you should either combine them with `@NotNull` or explicitly perform null checks within the validator itself.
## Internationalization of Messages
- By using message properties (e.g., `ValidationMessages.yml`), you can handle custom messages in multiple languages.
- Example message properties:
```yaml
NotSpace:
  message: 공백일 수 없습니다  # "Cannot be blank"
```

- Example annotation usage
```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = NotSpaceValidator.class)
public @interface NotSpace {
    String message() default "{NotSpace.message}";  // 🔹 Use key for i18n
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```