- https://www.learncpp.com/cpp-tutorial/introduction-to-programming-languages/
- Source code ---(compiler)--->  assembly code ---(assembler)---> object files (machine code + metadata + etc)   ---(linker)--> executable
- machine code
	- set of `0`  and `1`s (binary)
	- consists of instructions that tell the CPU what operations to perform
	- the low-level binary code that the CPU can directly execute (not an executable)
- object files
	- `.obj` or `.o` files 
	- contain machine code along with metadata and information about symbols
	- typically not directly executable
- linker
	- takes one or more object files and combines into an executable
	- resolves references between object files, includes necessary libraries, and sets up the executable's entry point
- Compilers
	- Compiles source code written in high-level language into assembly
	- modern c++ compilers often includes the assembler so it just compiles directly into machine code
	- some modern compilers
		- clang
		- gcc
- Interpreters
	- interpreter is a program that directly executes the instructions in the source code without requiring them to be compiled into an executable first


- CPUs are only capable of understanding machine language
	- Each CPU family has different syntax for assembly/machine code depending on the CPU family
	- Each CPU family has its own architecture and instruction set architecture (ISA) that defines assembly/machine code
	- ex. `x86` uses commands like `mov`

### Options in IDE
- **Build** compiles all _modified_ code files in the project or workspace/solution, and then links the object files into an executable. If no code files have been modified since the last build, this option does nothing.
- **Clean** removes all cached objects and executables so the next time the project is built, all files will be recompiled and a new executable produced.
- **Rebuild** does a “clean”, followed by a “build”.
- **Compile** recompiles a single code file (regardless of whether it has been cached previously). This option does not invoke the linker or produce an executable.
- **Run/start** executes the executable from a prior build. Some IDEs (e.g. Visual Studio) will invoke a “build” before doing a “run” to ensure you are running the latest version of your code. Otherwise (e.g. Code::Blocks) will just execute the prior executable.

### Configuring your compiler: Build configurations
> Use the _debug_ build configuration when developing your programs. When you’re ready to release your executable to others, or want to test performance, use the _release_ build configuration.
- Build configuration (build target)
	- A collection of project settings that determines how your IDE will build your project
	- What the executable will be named, that directories the IDE will look for in other code and library files, how much to have the compiler optimize, etc
- debug configuration
	- designed to help you debug your program, and is generally the one you will use when writing your programs
	- turns of all optimizations and includes debugging info
	- programs much slower but much easier to debug
- release configuration
	- used when releasing your program to the public
- FOr VSCode
	- Above the “${file}” line, add a new line containing the following command (one per line) when debugging: "-ggdb"
	- Above the “${file}” line, add new lines containing the following commands (one per line) for release builds:
		"-O2",
		"-DNDEBUG",

