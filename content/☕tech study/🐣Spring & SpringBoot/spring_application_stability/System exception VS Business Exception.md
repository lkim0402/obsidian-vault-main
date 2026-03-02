
>[!Overview]
>A business exception is different from a system error (like a `NullPointerException`). It represents a situation where a **domain or business rule has been violated**.

- For example, situations like "This user has already canceled their membership" or "There is not enough stock to complete the order" are not technical system failures => they are expected exceptions within the application's business logic
- [[Exception handling]]

|Aspect|System Exception|Business Exception|
|---|---|---|
|**Meaning**|A technical failure, programming mistake, or infrastructure problem.|A violation of an established business rule.|
|**Examples**|`NullPointerException`, `IOException`, database connection failure.|`DuplicateOrderException`, `InvalidUserStateException`, `InsufficientFundsException`.|
|**Handling**|The operation usually cannot be recovered. The error should be logged, and a generic failure response (e.g., `500 Internal ServerError`) is sent.|The operation is stopped, and a specific, meaningful message is returned to the user (e.g., `409 Conflict`, `400 Bad Request`).|
|**Origin**|Often originates deep within a framework, library, or low-level code.|Originates in your application's **business service layer**.|
# Business Exception
- location - [[⭐Layered Architecture#`@Service`|service layer]]
- It's standard practice to design business exceptions as **unchecked exceptions** by having them extend `RuntimeException`. This keeps your business logic clean from mandatory `try-catch` blocks.
- It's a good idea to separate the exception messages used for **internal logging** from the messages shown to the **end-user**. The internal log can be very detailed for debugging, while the user message should be simple and clear.
- Use [[Exception handling#⭐Custom Exceptions|⭐Custom Exceptions ]]
# Example - business exception
```java
public class OrderService {
    public void placeOrder(String productId, int quantity) {
        Product product = productRepository.findById(productId);

        // Check the business rule: Is there enough stock?
        if (product.getStock() < quantity) {
            // If not, throw a specific business exception.
            throw new BusinessException(ErrorCode.OUT_OF_STOCK);
        }

        // ... continue with order logic if stock is sufficient.
    }
}
```