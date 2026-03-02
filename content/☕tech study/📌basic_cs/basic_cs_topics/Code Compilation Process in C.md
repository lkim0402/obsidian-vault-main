
>[!Compiler]
>Translates source code (written in programming languages) into machine code (0s and 1s)

# `make` command
- `make` is an automation tool, not the compiler itself. It simplifies the build process by running the actual compiler (like `clang`) with the correct commands and arguments for you
- `make` vs. `clang`
	- **Using `clang` directly (The manual way)**
	    - `clang hello.c`: This works, but it outputs a generic file named `a.out` (assembly output), and you lose your desired program name
	    - `clang -o hello hello.c`:  The `-o hello` flag tells `clang` to create an output file named `hello` from the source file `hello.c`
	    - `clang -o hello hello.c -lcs50`: You also need to explicitly state libraries (`-l` indicates link)
	- **Using `make` (The automated way)**
	    - `make hello`:  Automatically runs `clang -o hello hello.c`, also no need to state libraries
- Note about libraries
	- C standard library
		- The default toolbox that comes with every C compiler, and the linker *always* checks this toolbox automatically
		- When using `clang` you don't need to specify with `-l`
		- Ex. `stdio.h` 
	- External libraries
		- Need to specify with `-l` with `clang`
		- Ex. `cs50.h`
# The steps of "compiling"
- Saying that `make` command "compiles" your code is abstracting a 4-lvl process, and we just call the entire process "compiling"
## Preprocessing
- Main steps
	1. The preprocessor directive `#include <stdio.h>` finds the header file `stdio.h`
		- the `.h` file almost entirely just contains function prototypes (declarations) & not the full function logic.
	2. It "copy-pastes" the entire text content of that `stdio.h` file into your `.c` file
- **Preprocessor directives**
	- a specific command to the preprocessor (the program that runs right before the main compiler), starts with `#`
	- `#include` - Find the file named after this, and *copy-paste its EENTIRE contents* right here in my code, effectively replacing the `#include` line
	- `#if`, `#else`, `#endif` are other preprocessor directives
- **Header file**
	- a real file on the computer that ends with a `.h` extension, contains prototypes/declarations
	- Ex) `<stdio.h>`
		- stands for "Standard Input/Output Header" - contains the declarations for functions like `printf()` and `scanf()`
##  (Actual) Compiling
- Gets the preprocessed code & translates it to assembly code
## Assembling
- convert the assembly code to binary (`0`s and `1`s)
## Linking
- Like the final step that assembles the complete puzzle
- After the compiler turns your `hello.c` into a machine-code "object file" (`hello.o`), that file has "gaps" or "placeholders" for any functions you used but didn't write, like `printf()`
- Steps
	1. The linker looks at those function calls (ex. `printf()`) 
	2. It finds the actual, pre-compiled machine code for those functions in the C standard library
	3. It "stitches" that library's machine code together with your machine code.
	4. The result is **one single, complete, runnable executable file**
- Other things to know
	- **The `-o` Flag:**  `-o` simply tells the linker what to output the final executable file as (e.g., `clang -o hello hello.c`).
	- **Decompiling:**  Reversing this process (turning the final machine code back into C) is called "decompiling," and it's extremely difficult and ambiguous because much of the original source code's structure is lost

📌Decompiling/Reversing the process?
- Can you actually decompile/reverse the process? If you have only `0`s and `1`s, can you make it back to source code?
- While you *technically can*, it becomes harder and more ambiguous
	- Ex. You wouldn't know if you used a for loop, a while loop, etc