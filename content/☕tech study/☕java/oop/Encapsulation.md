
>[!definition]
>The OOP principle of hiding internal details (data) and controlling access to them

- It bundles related attributes and functionalities into a single unit within a specific object, while protecting the data from external access
- By allowing data to be accessed only through the methods provided by the object, it achieves two goals: data protection and preventing unnecessary external exposure
- Summary
	- Restricts direct external access to an object's attributes.
	- Allows access to data through methods that can validate its integrity.
	- Lowers coupling between objects, thereby improving maintainability.
- Understanding [[Access modifiers (접근 제어자)]] is crucial for encapsulation

# Encapsulation in Java
1. **Making fields `private`** (hidden from outside)
	- making fields directly inaccessible from outside 
2. **Providing `public` getters and setters** to access and modify them
	- Core part of encapsulation
	- You can **validate** or **restrict** how data is accessed or changed
		- `setter` -  you can set conditionals/validation logic
	- You hide implementation details
	- Only use setter and getter when necessary
		- you can only provide getters or setters only