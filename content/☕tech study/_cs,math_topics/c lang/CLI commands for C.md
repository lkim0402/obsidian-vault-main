> How do we get from source code to machine code? How does a command like make work?

# Common commands
```bash
code hello.c # create file 
make hello # run the "compiler" to convert to machine code
./hello # execute/run the program
```
- `code hello.c`
	- makes a new `.c` file
- `make hello`
	- creates a file named `hello` (no extension) -> converts from source to machine code
- `./hello`
	- runs the code
- Other commands
	- [[Bash commands]]
## more about `make`
- `make` is not actually a compiler, but a useful program that automates the process of running an actual compiler for you (`clang` for us in this case)
- when we're using `make hello`:
	- make runs `clang`, and the output by default is `a.out` (assembly output)
	- so if you use `clang` directly, you don't get a program called `hello` but you get `a.out`
```bash
make hello
ls # hello
./hello

clang hello.c
ls # a.out
./a.out
```

# Command line arguments
> Inputs provided to commands at the command line
```bash
code hello.c
make hello 
./hello
```
- `make hello`
	- source to machine code manually
	- triggers the compilation
	- specifies what program to make
- `./hello`
	- run the machine code
## `make` to `clang`
```bash
clang -o hello hello.c -lcs40 
./hello
```
- arguments
	- `-o hello hello.c`
		- `-o` - output
		- outputs the default `a.out` name to `hello`
	-  `-lcs50` 
		- using 3rd party libraries:`l` stand for link against a library
		- When using `clang` or `gcc` directly, you **must specify** all needed libraries manually — which can be tedious.
		- this is why `make` is more convenient, it does all these under the hood
## `argc` and `argv`
- If we want to use CLI at runtime instead of while the program is running, we can change our `main` method to look like this
```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
    if (argc == 2)
    {
        printf("hello, %s\n", argv[1]);
    }
    else
    {
        printf("hello, world\n");
    }
}
```
-  `argc` - argument count
	- stores the number of command line arguments the user typed when the program was executed
	- `./greedy` - argc == 1
	- `./greedy 100` - argc == 2
- `argv[]` - argument vector
	-  array of the characters passed as arguments at the command line
	- stores **strings**
		- so even tho you typed ints in the CLI it is a string
	- first element is `argv[0]` and last is `argv[argc-1`