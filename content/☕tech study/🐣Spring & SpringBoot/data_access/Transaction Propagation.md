>[!Overview]
>**Transaction propagation** is a setting that determines whether a sub-method will use a new transaction or participate in an existing one when a transaction is already in progress. 
>- In [[🐣Spring & SpringBoot|Spring]], it's configured via the `propagation` attribute of `@Transactional`, with **`REQUIRED`** as the default.
>- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)|Declarative vs. Programmatic Transaction - Declarative Transaction (@Transactional)]]

- Transaction propagation settings are important for the following reasons:
	- Maintaining the consistency of business logic
	- Controlling the scope of rollbacks when exceptions occur
	- Optimizing performance
	- Controlling concurrency
# Managing Multiple Operations as One Transaction

## Before (independent)
![[transaction-propagation.png]]
- `orderService.createOrder()` -> saves order info
- `memberService.updateStamp()` -> update stamp
- If these two transactions are executed independently like this
	- if an exception happens during the `updateStamp()` operation: **the order information will be saved, but the stamp count will not be updated.**
## After (single transaction)
![[transaction-propagation2.png]]
- Now, if an exception occurs while processing `memberService.updateStamp()`, all operations in both classes will be rolled back since they are within the same transaction boundary
- In the diagram you can see that the individual transaction boundaries are now connected into one

# Example Code 
OrderService
```java
@Transactional  // (1)
@Service
public class OrderService {
    ...

    public Order createOrder(Order order) {
        verifyOrder(order);
        Order savedOrder = saveOrder(order);
        updateStamp(savedOrder); // <<<<<

		// (2)
        throw new RuntimeException("rollback test");
//        return savedOrder;
    }

    private void updateStamp(Order order) { // <<<<
        Member member = memberService.findMember(order.getMember().getMemberId()); // 3
		...

        memberService.updateMember(member); // 4
    }

    private Order saveOrder(Order order) {
        return orderRepository.save(order);
    }
	...
}
```
1. `@Transactional` on the class -> `@Transactional` applied to all public methods like `createOrder()`
2. we throw new error for testing
3. We call `memberService.findMember` (has `@Transactional`)
4. We call `memberService.updateMember` (has `@Transactional`)

MemberService
```java
@Transactional
@Service
public class MemberService {
    ...

     // (1)
    @Transactional(propagation = Propagation.REQUIRED)
    public Member updateMember(Member member) {
        Member findMember = findVerifiedMember(member.getMemberId());

		...
        return memberRepository.save(findMember);
    }

    @Transactional(readOnly = true)
    public Member findMember(long memberId) {
        return findVerifiedMember(memberId);
    }
    
	...
}
```
1. `updateMember(Member member)`
	- has `propagation = Propagation.REQUIRED`
	- this ensures that if a transaction is alr in progress when the method is executed, the method will use that existing transaction (if none exists a new one will be created)
	- So, when `createOrder()` from `OrderService` is called, a new transaction is created -> then when `udpateMember()` is called from within the `createOrder()` method, it participates in this alr existing transaction
- If you test `postOrder` to the [[⭐Spring MVC#Controller|controller]], the exception has occurred
	- both `saveOrder` and `updateStamp` (which contains calls to `memberRepository`) are rolled back
# Transaction propagation options

|Option|Description|Use Case / Example|
|---|---|---|
|**`REQUIRED`**|Joins an existing transaction, creates a new one if none exists.|Most standard business logic.|
|**`REQUIRES_NEW`**|Always starts a new, independent transaction.|Audit logs, saving history records.|
|**`SUPPORTS`**|Joins an existing transaction, executes non-transactionally if none exists.|Read operations, simple logging.|
|**`MANDATORY`**|Requires an existing transaction, throws an exception if none exists.|Logic that must run within a parent transaction.|
|**`NEVER`**|Throws an exception if a transaction exists, executes non-transactionally otherwise.|Calling external APIs that don't support transactions.|
|**`NOT_SUPPORTED`**|Suspends the current transaction and executes non-transactionally.|Non-critical logging or update tasks.|
|**`NESTED`**|Creates a nested transaction using a database Savepoint if a transaction exists.|Partial rollback during large-scale data processing.|