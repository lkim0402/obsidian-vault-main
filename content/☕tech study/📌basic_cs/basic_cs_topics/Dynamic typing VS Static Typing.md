- **Data type**
	- A data type in a programming language is a characteristic that defines the nature of the value that a data element has
- **Type checking**
	- A system of checking if values has been assigned their correct types
	- Essential in programming to minimize errors during the execution of programs
	- Every programming language has this
	- Usually occurs at either *runtime* or *compile time*
	- The two main ways of type checking in most languages are *static* and *dynamic*
- [[]]
- Diagram
	- ![](https://www.baeldung.com/wp-content/uploads/sites/4/2021/10/static_dynamic.drawio.svg)
# Dynamically Typed
>[!Overview]
 A language is dynamically typed if the type is associated with run-time values, and not named variables/fields/etc
- You don't declare variable types explicitly, but the type is *inferred* at **runtime** based on the assigned value.
	- Variables are checked against types only when the **program is executing**!
	- [[Python]], [[JavaScript]]
- Characteristics
	- No requirement to explicitly state the data type during variable declarations, since the language system can detect the type of variable at runtime
		- The computer decides, so efficiency is low
	- Altering the data type of previously declared variables is allowed
- Convenience and readability - easier to understand

# Statically Typed
>[!Overview]
>A language is statically typed if the type of a variable is known at compile time.
- You *explicitly declare variable types*, or the compiler infers them at **compile time**.
	- Compile time is when a specific programming language is converted to a machine-readable format
	- This means that before source code is compiled, the type associated with each and every single variable must be known
	- [[☕Java]], [[C++]]
- Characteristics
	- Detection of errors in variable-data type associations will halt the compilation process until error is fixed!
	- A variable declared with a specific data type cannot be assigned to another later
- Detailedness, lots of safety devices, prevents mistakes

# A Comparison Chart

| Feature        | Dynamic                                  | Static                                                   |
| -------------- | ---------------------------------------- | -------------------------------------------------------- |
| Type Checking  | At runtime                               | At compile time                                          |
| Variable types | Determined when code runs                | Declared before running                                  |
| Flexibility    | More flexible, less boilerplate          | Stricter, but safer                                      |
| Errors         | May fail at runtime (type errors)        | Type errors caught early (before running)                |
| Performance    | Usually slower                           | Usually faster                                           |
| Scope          | Better for fast, small personal projects | Better for big projects that needs lots of collaboration |
| Examples       | [[Python]], [[JavaScript]]               | [[C++]], [[☕Java]]                                       |