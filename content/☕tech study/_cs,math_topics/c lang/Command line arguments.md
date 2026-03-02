- `Command-line arguments` 
	- arguments that are passed to your program at the command line
	- Ex) all statements you typed after `clang` are considered command line arguments -> You can use these arguments in your own programs!

Example
```c
// Prints a command-line argument
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
- `int argc, string argv[]`
	- `argc`:  The number of command line arguments
		- stands for argument count
	- `argv`: An array of the characters passed as arguments at the command line
		- stands for argument vector (often another word for array)
    
- You can print each of the command-line arguments with the following:
```c
// Prints command-line arguments

#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
	for (int i = 0; i < argc; i++)
	{
		printf("%s\n", argv[i]);
	}
}
```