> Refers to how the language treats the types of variables
- Related: [[Compile time vs runtime]]
### Static
- each variable stores a value of a specific type and the type needs to be known ahead of time
-  cannot reassign types 
```python
int x = 3;
x = "hello"; // fails to compile because x can only hold an integer

// still static because the compiler will still have to figure out the type during compile time
x = 3;
x = "hello"; // this still fails to compile because x can only hold an integer
```
### Dynamic
- You **don't declare the type of the variable**
	- We can reassign different **types** of variables
- Python, JS
- The types don't need to be known at compile time although they can be checked at runtime
```Python
x = 3 # x holds an int
x = "hello" # now x holds a string
```

