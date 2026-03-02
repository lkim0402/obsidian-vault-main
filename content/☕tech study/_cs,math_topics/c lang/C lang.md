- [[Code Compilation Process in C]]
- [[Writing first program in C]]
- [[Debugging C]]
# Syntax
- [[Data types, variables, operators - C]]
- [[Arrays and strings - C]]
- [[Loops - C]]
- [[Conditionals - C]]
- [[Magic numbers]]
- [[Format codes]]
- [[Command line arguments]]
# Build Tools
- debug50
- [[CLI commands for C]]
- [[Code Compilation Process in C]]
- [[functions in C]]



- passed by value
	- local variables in C are passed by value in function calls
	- the **callee** receives a **copy of the passed variable**, not the variable itself
	- the variable in the **caller** is unchanged unless overwritten
- passed by reference
	- arrays
	- the callees **receive the actual array**, not a copy of it
	- then the callee manipulates the elements of the array, the actual array's elements will ALSO be manipulated
