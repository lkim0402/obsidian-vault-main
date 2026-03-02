
>[!Overview]
>A ***composite object*** refers to a structure where an object contains other objects as fields or includes collections (such as List, Set, Map) of objects. 
>- When validation is needed, it’s often necessary to validate not only individual fields but also such nested structures.
>- Use `@Valid`

- `@valid`
	- Since `@Valid` propagates validation recursively, deeper nested structures can impact performance. It’s best to apply it only when necessary.
	- `@Valid` vs `@Validated`: For nested object validation itself, `@Valid` is sufficient; however, if you need additional features like group validation, use `@Validated`.
# Example 1: User and Address
```java
// Address.java
public class Address {
    @NotBlank(message = "City must not be blank")
    private String city;

    @Pattern(regexp = "\\d{5}", message = "Zip code must be 5 digits")
    private String zipCode;

    // getters and setters omitted
}
```

```java
// User.java
public class User {
    @NotBlank(message = "Name must not be blank")
    private String name;

    @Valid  // Must be added on nested objects to trigger validation of inner fields
    private Address address;

    // getters and setters omitted
}
```
- Applying the `@Valid` annotation on the nested object field (`Address address`) triggers validation of the fields *inside the `Address` object*.
- Without `@Valid`, only the presence (e.g., null check) of the `address` field itself is validated in the `User` object, and internal fields like `city` and `zipCode` are ignored.
# Example 2: Order and Product
```java
// Product.java
public class Product {
    @NotBlank(message = "Product name must not be blank")
    private String name;

    @Positive(message = "Price must be greater than 0")
    private int price;

    // getters and setters omitted
}

// Order.java
public class Order {
    @NotBlank(message = "Customer is required")
    private String customer;

    @Valid  // Propagate validation to each element
    @Size(min = 1, message = "At least one product must be included")
    private List<Product> productList;

    // getters and setters omitted
}
```

- The `@Valid` annotation propagates validation to each element in a collection type (List, Set, Map, etc.).
- Constraints that apply to the collection as a whole (e.g., size restrictions) can be enforced using annotations like `@Size`.
- In this example, if `productList` is null or its size is 0, a validation error will occur.
- Additionally, each `Product` inside the list will be validated for its own constraints on `name` and `price`.