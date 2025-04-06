- [[Linux]]

| **Concept**                 | **Description**                                                                                                                                                                                        | **Role**                                                                                                                                                                                          | **Example**                              |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| **Bash**                    | - A **shell program** that interprets the commands you type in the **command line** and executes them on your system.<br>- It's a **command-line interface** and a programming language for scripting. | **Handles the input from the command line** and **talks to the kernel** to execute your commands                                                                                                  | `echo "Hello, World!"`                   |
| **Shell**                   | A program that interprets and runs text commands, like Bash, Zsh, or Fish.                                                                                                                             | Provides a **command-line interface** between the user and the OS.                                                                                                                                | Bash, Zsh, Fish                          |
| **Terminal** & Command line | Terminal - A software program that provides an interface for interacting with the shell using text commands.<br><br>Command line -The literal text area where you type commands.                       | Terminal - A **window** or interface where you can run commands (contains the command line and shell).<br><br>Command line - The input field where commands are typed and processed by the shell. | Terminal: GNOME Terminal, macOS Terminal |
| **[[kernel]]**              | The core part of the [[Operating systems (OS)\|operating system]] that interacts with hardware (CPU, RAM, devices, etc.).                                                                              | **Manages system resources** like memory, processes, and hardware interaction.                                                                                                                    | Linux Kernel, Windows NT Kernel          |

- **Bash** is a **shell** used to interpret commands.
- The **shell** is the program that runs in the terminal and executes commands.
- The **terminal** is where you interact with the shell.
	- The **command line** is just the text-based interface where commands are typed (part of the terminal)
- The **kernel** handles hardware and low-level system tasks.

## General flow
1. **You open the terminal** → The terminal program runs.
2. The terminal **starts a shell** (usually **Bash**).
3. **The shell (Bash)** displays the **command line** (prompt) where you can type your command.
4. You type a command, like `ls`, in the **command line**.
5. **Bash (the shell)** processes the command, sends it to the **kernel** if necessary (processes system calls and executes it, then gives control back to shell), and receives the result.
6. The **result** is displayed back in the terminal below the command line.