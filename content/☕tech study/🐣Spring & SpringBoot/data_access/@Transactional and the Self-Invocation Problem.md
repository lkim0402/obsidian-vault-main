- [[AOP (Aspect Oriented Programming)]]
- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)|Declarative vs. Programmatic Transaction - Declarative Transaction (@Transactional)]]
# Why Internal Calls Fail
- Spring uses **proxies** to manage transactions. 
	- When you call a method in a Spring bean from _another_ class, you are actually **calling a proxy object that Spring created**. 
	- This proxy is what starts, commits, or rolls back the transaction before and after your actual method code runs.
## Problem: Self-invocation
- The problem occurs when a method inside a class calls another method with `@Transactional` **in the same class**. This is called **self-invocation**.
- In this case the call doesn't go through the proxy, it's a direct `this.method()` call within the object itself
- Because the proxy is bypassed, Spring's transaction logic never gets a chance to run
```java
@Service
public class OrderService {

    // 1. This is the OUTER method (no @Transactional)
    public void placeOrder() {
        System.out.println("Outer method starts.");
        // 2. This is a self-invocation call. It bypasses the proxy.
        this.updateInventory(); // The @Transactional here will be IGNORED.
        System.out.println("Outer method ends.");
    }

    // 3. This is the INNER method
    @Transactional
    public void updateInventory() {
        // This code will NOT run in a transaction.
        System.out.println("Updating inventory...");
    }
}
```
- When `placeOrder()` is called from another service, it's not transactional. The subsequent call to `this.updateInventory()` is a direct internal call, so the `@Transactional` annotation is completely ignored.
## Problem: Transactions are merged
- Situation: Both the outer and inner methods are annotated with `@Transactional`
	- By default, if a transactional method calls another transactional method, the inner method **joins the existing transaction** of the outer method. They become ***one single transaction***!
- So basically
	- Even if you try to start a new transaction, it gets merged. 
	- Trying to roll back only the inner transaction using `REQUIRES_NEW` won't work => it will **still be ignored** because of the self-invocation problem
	- The inner method's transaction doesn't get applied.
```java
@Service
public class OrderService {

    // 1. OUTER method starts a transaction (TX 1)
    @Transactional
    public void placeOrder() {
        // ... some database logic ...

        // 2. Self-invocation call. The annotation below is IGNORED.
        // It does NOT start a new transaction. It just continues in TX 1.
        try {
            this.processPayment();
        } catch (Exception e) {
            // This catch block won't help isolate the rollback.
        }

        // ... more logic ...
    }

    // 3. INNER method. The goal is a separate transaction, but it fails.
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void processPayment() {
        // This code runs inside the outer method's transaction (TX 1).
        // If it throws an exception, the ENTIRE placeOrder() transaction will be marked for rollback.
        throw new RuntimeException("Payment failed!");
    }
}
```
- Even though `processPayment` is marked `REQUIRES_NEW`, it doesn't create a new transaction. The exception from `processPayment` will cause the entire `placeOrder` transaction to roll back.
## Solution - Separate into another bean (class)
- To make this work, you must move the inner method into its own, separate [[Bean|Spring Bean]] (e.g., another `@Service`).
- When you call the method from the new bean, you are going through its *proxy*, allowing Spring to apply the transaction rules correctly.
**1. The New, Separated Service:**
```java
// The new, separate bean
@Service
public class PaymentService {

    // This method will now correctly start its own, new transaction.
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void processPayment() {
        System.out.println("Processing payment in a new transaction.");
        // ... payment logic ...
        throw new RuntimeException("Payment failed!"); // This only rolls back the payment transaction.
    }
}
```
**2. The Original Service, Calling the New Bean:**
```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    // Inject the new service
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    // Outer method starts its transaction (TX 1)
    @Transactional
    public void placeOrder() {
        System.out.println("Placing order in transaction 1.");
        // ... some database logic for the order ...

        try {
            // This is NOT a self-invocation. It calls the proxy of PaymentService.
            // Spring will suspend TX 1 and start a new transaction (TX 2) here.
            paymentService.processPayment();
        } catch (RuntimeException e) {
            // The payment failed and TX 2 was rolled back.
            // But TX 1 (the order) is still active and can be committed.
            System.out.println("Payment failed, but we can still save the order.");
        }

        System.out.println("Committing order transaction (TX 1).");
    }
}
```
- Now, because the call is to a different bean (`paymentService`), it goes through the proxy, and `Propagation.REQUIRES_NEW` works as expected. The payment can fail and roll back independently without affecting the main order transaction.
- The old transaction gets suspended (paused) and is resumed later
