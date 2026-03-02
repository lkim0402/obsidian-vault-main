
> [! C++  is a compiled language.]
>- source files (code you write) -> compiler "translates" them into machine code (binaries)
>	- every `.cpp` file gets compiled individually into an object file (`.obj)
>	- compile `Ctrl + Shift + B`
>	- then the linker takes all the `.obj` files and "glues" them together into one `.exe` file
>- then you execute the file via terminal

- C++ comes with an extensive library called the **C++ Standard Library** (usually called the standard library) that provides a set of useful capabilities in your programs

![[Pasted image 20250120215254.png|500]]

- `g++`
	- Both COMPILER AND LINKER
- Output executable w specific name `myprogram`
	- `g++ file.cpp -o myprogram`
		- `g++`: The C++ compiler
		- `file.cpp`: The C++ source file you're compiling
		- `-o myprogram`: The name of the output executable (machine code)
			- g++ takes `file.cpp` and generates object file (`file.o`) 
				- contains machine code but not complete executable
			- g++ then after compiling, links `file.o` with any libraries you may have specified, creating final executable `myprogram` (without any extension)
				- fully linked and complete program
		- So the `g++` compiler compiles the source code of `file.cpp` into an executable object file `myprogram`
- Output executable w/o specific name (defaults to `a.out`)
	- `g++ file.cpp`
	- since there are no `-o` flags, the default output file name is `a.out` (machine code)
- Running the executable `myprogram` (or `a.out`)
	- `./myprogram`
	- this executable file will then  be loaded o computer memory, and the computer's CPU will execute the program instruction at a time
- `Ctrl + Shift + B`
	- Automatically builds for us
### Some Visual Code configs
- Solution config and Solution platform
	- Solution config - Debug and Release
	- Solution platform - x64, x86 
- Imp stuff to check
	![[Pasted image 20241105190603.png]]