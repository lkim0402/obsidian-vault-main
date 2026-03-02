
>[!Ocp]
>The design principle that states: "software entities (classes, modules, functions, etc.) should be **open for extension, but closed for modification**."

- You should design your code so that new functionalities can be added through extension **without altering existing code**. 
- If new requirements always lead to modifications of existing classes, it increases the risk of errors and raises maintenance costs.
#### Bad Example
```java
class PaymentService {

    void pay(String method) {

        if (method.equals("card")) {
            System.out.println("카드 결제");
        } else if (method.equals("kakao")) {
            System.out.println("카카오페이");
        }
    }
}
```
- Every time a new payment method emerges, the `pay()` method must be modified (continuously growing `if-else-if` statement structure). 
- The class remains constantly exposed to changes, thus violating OCP.

#### Good example
```java
interface PayStrategy {

    void pay();
}

class CardPay implements PayStrategy {

    public void pay() {
        System.out.println("카드 결제");
    }
}

class KakaoPay implements PayStrategy {

    public void pay() {
        System.out.println("카카오페이");
    }
}

class PaymentService {

    private PayStrategy payStrategy;

    public PaymentService(PayStrategy payStrategy) {
        this.payStrategy = payStrategy;
    }

    public void processPayment() {
        payStrategy.pay();  // Behavior changes based on the implementation
    }
}

///(lecturer code)
class PayApplication {
	psvm(String[] args) {
		PayStrategy pg = new KakaoPay();
		PaymentService ps = new PaymentService(pg);
		ps.processPayment(); // Payment via KakaoPay
	}
}
```
- Dependency Injection (e.g., Spring): The PaymentService's decision-making power resides externally.
- By creating a new class for each payment method, we enable extension.
- The existing PaymentService does not need any modifications.
