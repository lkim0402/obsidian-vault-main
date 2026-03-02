- One of the primary ways to interact with your computer
- Shell VS Bash
	- The **shell** is the program that runs in the terminal and executes commands.
	- **Bash** is the name of one particular shell. -> `Bourne Again SHell`
		- [[Bash commands]]
		- [[Bash, shell, terminal, command line, kernel]]
-  The Shell as a Programming Environment
	- The shell is more than just a command prompt; it's a complete programming environment, similar to Python or Ruby. It includes features like variables (e.g., `$PATH`), conditionals, loops, or functions!
	- When you type a command, you are *writing a small piece of code for the shell to interpret*.
# How the Shell Finds and Executes Programs
## Environment variable
- things that are set whenever you start your shell
	- bunch of things that's already set
- Example: `$PATH`
## Build in / External Commands
- [[Bash commands]]
- built-in
	- already part of bash so no need to search the filesystem for a program
	- Ex) `cd`, `alias`,  `export`, `source`
	- NOT UNDER `$PATH`
- External (Not built in)
	- Separate programs on your computer. -> Each one is its own executable file living in a directory like `/bin` or `/usr/bin`
	- Ex) `ls`, `grep`, `find`, `date`, `cat`, `echo`
	- Uses `$PATH` <<<<< 
- Note: `echo` exists as _both_ a shell built-in and an external command (`/bin/echo`)
- You can check if its built in or not: `type <COMMAND>`, like `type echo`
##  `$PATH` 
- How does the shell know where to find programs that are external?
	- The shell consults an **environment variable** called `$PATH`
	- `$PATH` contains a list of directories, separated by colons (`:`), where the shell should look for executable programs.
	- Essentially, `$PATH` is the roadmap the shell uses to locate commands.

```bash
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin # same as above
```

- built in commands are NOT HERE
	- If a command is built in, the shell finds and executes it before it even considers looking at `$PATH`
- `which`
	- tells you the exact location (the full path) of the program that the shell will execute
	- ex) `/bin/echo` is where the `echo` program is
- You can run a program without relying on `$PATH` by providing its full, direct path. This guarantees you are running that specific executable
	- `/bin/echo "hello world"`
	- But typically you usually don't need to type `/bin/echo` because the `echo` program's location is already listed in your `$PATH` variable 
#### What happens
1. **Check for an alias**: It first checks if the command is an alias (a nickname you've set for a longer command).
2. **Check for a built-in command**: If it's not an alias, it checks if the command is one of its **built-in** functions. If it is, it runs it immediately **without using `$PATH`**.
3. **Search `$PATH`**: If the command is not a built-in, the shell then searches through the directories listed in **`$PATH`** to find a matching **external program**.
4. **Error**: If it can't find the command after all these steps, it will give you the `command not found` error.