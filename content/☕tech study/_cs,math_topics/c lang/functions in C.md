# Exit status
- When a program ends, a special exit code is provided to the computer.
	- `main` function returns an int
- technically programs can also secretly return numbers to signal methodically whether the program was successful or not
	- status code 0 ->program exits without error
	- status code 1 ->when an error occurs that results in the program ending
- `echo $?`
	- shows the exit status of the most recent command

# Declaration vs Definition
- function declaration
	- `return type, name, arguments.;`
- function definition
	- actual implementation of the function 
# Order of functions & prototypes
```c
void meow(void); // you need to add the prototype like this

int main(void) {
	for (int i = 0; i < 3; i++) {
		meow();
	}
}

void meow(void) {
	print("meow");
}
```
- C can only read from top to bottom
- So you need to add the *prototype* of your function on the top
	- 1st line of the function
	- "hey, there is a function called meow and it will eventually exist"