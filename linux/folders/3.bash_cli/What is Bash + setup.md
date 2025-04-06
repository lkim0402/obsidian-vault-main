
> "Bourne Again Shell"

- Useful
	- [[Bash, shell, terminal, command line, kernel]]
- Also called a shell  because it surrounds the OS's kernel to hide its intricate details
- When you open the terminal on mac/linux, the default shell is usually bash
- it provides a prompt where you type a command, which will be interpreted by the shell and executed on the [[Operating systems (OS)|operating system]]
- To check if ur using bash
	- `which $SHELL`
- It is also a programming language that allows us to write scripts!
	- when you first launch the shell, it actually runs a startup script that's defined in the `.bashrc` or `.bash_profile` file on your system
		- allows behavior or appearance of your shell
	- You can add your own custom bash scripts to any project by creating any file with `.sh` extension or no extension at all

- sources
	- fireship 100s - https://www.youtube.com/watch?v=I4EWvMFj37g

# Setup
- Before everything, make sure you're running a Bash
	- `echo ${BASH_VERSION}`
	- if it doesn't output anything, launch bash with just typing `bash`
- You can right click > Preferences, then customize