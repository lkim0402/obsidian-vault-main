
>[!LSP]
>Subclasses should always be able to substitute for their parent classes.
>-  That is, the principle states that when a **subtype (child class)** is used in place of its **supertype (parent class)**, the program's behavior should **remain unchanged**.

- A responsibility is synonymous with a **role** or a **reason for change**. If a single class changes for multiple reasons, it indicates it has more than one responsibility.
- "Only one role"

# Contract (계약)
- A contract is a design approach that clearly defines and adheres to the **pre-conditions** (input conditions), **post-conditions** (output conditions), **invariants**, etc., of a method.
- 📌 LSP is the principle that "child classes must also uphold this contract."
	- Even if a subclass overrides a parent's method, it must not violate the existing contract.
# Example
#### Bad example
```java
class Bird {

    void fly() { System.out.println("날아간다"); } // Flies
}

class Ostrich extends Bird {

    void fly() { throw new UnsupportedOperationException(); }
}
```
An Ostrich is a Bird but cannot fly. This breaks the "ability to fly" contract of a Bird.
#### Good example
```java
class Report {

    String title;
    String content;
}

class ReportPrinter {

    void print(Report report) { ... }
}

class ReportSaver {

    void saveToFile(Report report) { ... }
}
```
Each class now has only one responsibility. The reasons for changes are clearly separated.

| Method                             | Explanation                                                 |
| :--------------------------------- | :---------------------------------------------------------- |
| Separate into functional classes   | Don't put multiple roles into a single class                |
| Separation of Concerns (SoC)       | Clearly separate UI, logic, repository, etc.                |
| Design for a single responsibility | Create a structure where there's only one reason for change |