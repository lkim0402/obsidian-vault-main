# WTH is JVM, JRE, JDK?
- diagram
	![[Pasted image 20250521215733.png]]
## Java Development Kit (JDK)
> a software development kit that provides a set of tools and libraries for developing Java applications
- essential for developers who want to write, compile, and run Java code
- you will need to download this when developing Java applications
## Java Runtime Environment (JRE)
> A package that provides the libraries JVM needs to run Java applications
- Program that executes compiled Java bytecodes
- Does not include development tools, making it suitable for users who *only need to run Java programs without developing them*
	- Install the JRE when you only need to run Java applications but not develop them
- subset of the JDK
## Java Virtual Machine (JVM)
- [[Java Virtual Machine (JVM)]]
- A theoretical computer whose machine language is the set of Java bytecodes
- Source
	- https://www.digitalocean.com/community/tutorials/difference-jdk-vs-jre-vs-jvm
# The compilation process
1. Developer writes the `.java` files in Java (ex. `MyProgram.java`)
2. Compilation
	- The Java Compiler (a program named `javac` ) reads your `.java` source file
	- It translates your code into **Java Bytecode** (Saved in a file with extension `.class`)
		- A set of instructions specifically designed to be read by [[Java Virtual Machine (JVM)]] 
		- It's not tied to any OS or hardware, and this is the core of Java's "write once, run anywhere" principle
		- The same `.class` file can run on any computer that has a compatible JRE
	- Ex. `MyProgram.java` becomes `MyProgram.class`
3. Running the program
	- To run the `.class` file you need the JRE