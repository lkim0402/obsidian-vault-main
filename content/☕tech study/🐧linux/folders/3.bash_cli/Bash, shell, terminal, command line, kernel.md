- [[🐧Linux]]

| **Concept**                     | **Description**                                                                                                                                                                                                                         | **Role**                                                                                                                                                     | **Example**                              |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------- |
| [[Shell]]                       | -A program that interprets and runs text commands (*command line interpreter*)<br>- Ex) Bash, Zsh, or Fish.<br>- program that takes your commands and passes them on to the [[kernel]] to be executed                                   | Provides a **command-line interface** between the user and the OS.                                                                                           | Bash, Zsh, Fish, PowerShell              |
| **Bash**                        | - a specific and very popular *name of a [[Shell]]*<br>- It's a **command-line interface** and a programming language for scripting<br>- often default shell for [[Linux Distributions]]                                                | **Handles the input from the command line** and **talks to the kernel** to execute your commands                                                             | `echo "Hello, World!"`                   |
| **Terminal** & **Command line** | `Terminal` <br>- A software program that provides an **interface** for interacting with the shell using text commands.<br>- A **window** or **interface**<br><br>`Command line`<br>- The literal *text area* where you type commands.   | `Terminal` <br>- where you actually interact with the shell<br><br>`Command line` <br>- The input field where commands are typed and processed by the shell. | Terminal: GNOME Terminal, macOS Terminal |
| **[[kernel]]**                  | The core part of the [[Operating systems (OS)\|operating system]] that interacts with hardware (CPU, RAM, devices, etc.).<br>- the first program that loads when you turn on your computer, and it has complete control over everything | **Manages system resources** like memory, processes, and hardware interaction.                                                                               | Linux Kernel, Windows NT Kernel          |
- **The command prompt**
	- the text displayed by the shell in the terminal that indicates it's ready to accept a command
		- it "prompts" you to speak
		- its what the computer prints (u don't edit this)
	- `leekim@my-laptop:~$`
# General flow
1. **You open the terminal** → The terminal program runs.
2. The terminal **starts a shell** (usually **Bash**).
3. **The shell (Bash)** displays the **command line** (prompt) where you can type your command.
4. You type a command, like `ls`, in the **command line**.
5. **Bash (the shell)** processes the command, sends it to the **kernel** if necessary (processes system calls and executes it, then gives control back to shell), and receives the result.
6. The **result** is displayed back in the terminal below the command line.