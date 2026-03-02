
>[!SRP]
>**SRP (Single Responsibility Principle)**, also known as the Single Responsibility Principle, is an object-oriented design principle stating that a class should have only one responsibility.

- A responsibility is synonymous with a **role** or a **reason for change**. If a single class changes for multiple reasons, it indicates it has more than one responsibility.
- "Only one role"
#### Bad example
```java
class Report {

    String title;
    String content;

    void saveToFile() { /* Save to file */ }
    void print() { /* Print to console */ }
}
```
In this example, responsibilities for data management, printing, and saving are all mixed within one class.
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