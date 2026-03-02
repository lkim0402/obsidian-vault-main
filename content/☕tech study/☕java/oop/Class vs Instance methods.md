
| Feature                        | Instance Method          | Class Method (Static)        |
| ------------------------------ | ------------------------ | ---------------------------- |
| Defined with `static`?         | ❌ No                     | ✅ Yes                        |
| Belongs to                     | An **object** (instance) | The **class itself**         |
| Can access instance variables? | ✅ Yes                    | ❌ No (only static variables) |
| Called using                   | `objectName.method()`    | `ClassName.method()`         |
- If we make the constructors private, we can prevent someone creating new instances

# Class methods
- **static methods belong to the class**, not to any specific object
	- So **inside a static method**, there is **no implicit reference (`this`) to any instance**. That’s why you can’t directly use instance members (fields or methods).
- Use class methods if u need to use a method without creating an instance
- Utility classes
	- Tie functions u use frequently into one class
- add `static` keyword
	- [[Non-Access modifiers (기타 제어자)#Static Members|static]]
- Examples
	- `Math.sqrt()`
	- `Integer.parseInt()`
