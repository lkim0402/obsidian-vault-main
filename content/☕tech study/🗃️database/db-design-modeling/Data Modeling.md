
>[!Definition]
>- Process of **abstracting real-world data** into a structured form
>- Foundation for **logical + structural [[Database Design]]**

-  You must extract the characteristics well
	- Modeling a cat -> cute, fat... if you do it like this, you can't tell it's a cat lol
	- A proper model would use objective attributes like `species`, `color`, `weight_kg`, or `date_of_birth`
- Data modeling must be *independent of technology*
	- You should design your database based on the business logic and the data itself, **without thinking about a specific programming language or framework** (like [[🐣Spring & SpringBoot|Java Spring]])
	- If you design your tables specifically for how one framework works, the database becomes dependent on that technology
- 💡 All modeling should be designed **flexibly**, considering the possibility of future changes.
# Main Components 
- Mental Models (think of it this way)
	- Entity = Noun (e.g. _User_, _Product_)
	- Attribute = Adjective (e.g. _name_, _price_)
	- Relationship = Verb (e.g. _orders_, _has_)

| Component             | Description                                        | Example (Membership System)                         |
| --------------------- | -------------------------------------------------- | --------------------------------------------------- |
| **Entity (엔티티)**      | An object or concept that can exist independently. | `Member`, `Order`, `Product`                        |
| **Attribute (속성)**    | A unique property that an entity possesses.        | `name`, `email`, `price`                            |
| **Relationship (관계)** | A connection between two or more entities.         | `Member-Order` (A member can place multiple orders) |
# Advantages

|Purpose|Description|
|---|---|
|**Abstraction (추상화)**|Transforms the complex real world into a simple model that can be represented with data.|
|**Simplification (단순화)**|Represents complex structures or processes in a reduced form using agreed-upon notation and language.|
|**Clarification (명확화)**|Removes ambiguity so that everyone can understand the information in the same way.|

# Main Steps
| Stage                   | Representation Method                | Example                                                            |
| ----------------------- | ------------------------------------ | ------------------------------------------------------------------ |
| **Real World**          | "A member signs up."                 | A customer enters their information on the site.                   |
| **Conceptual Modeling** | Defining the concept of a "Member."  | Attributes like name, age, email, join date.                       |
| **Logical Modeling**    | Structuring it as a `members` table. | Designing columns like `member_id`, `name`, `email`, `created_at`. |
- [[Data Modeling - 4 main steps]]