>[!What is globbing?]
>- Globbing is the process in which the shell (like Bash) automatically expands wildcard patterns into matching filenames before executing a command. 
>- It's basically a way to use shorthand symbols (wildcards) to match multiple files instead of listing them manually.
#### Globbing with *
```bash
ls *.txt
```
- `*` (asterisk) is a wildcard that matches any characters.
	- 0 to unlimited number of characters
- This command expands to list all files ending in `.txt` in the current directory. If you had `notes.txt` and `report.txt`, Bash would rewrite it as:
```bash
ls notes.txt report.txt
```

- `ls folder/*` will list everything in the folder
- if nothing matches then the entire term with the globbing will be treated as it's one file
- escaping the wildcard
	- `ls '*.txt'`
#### Additional globbing wildcards
- `?` 
	- matches any single character
		![[Pasted image 20250313115728.png]]
- `[0-9]`
	- a range, allowing us to specify a character range (here: all numbers)
	- `echo ./images/IMG?[0-9][0-9][0-9][0-9].[a-z][a-z][a-z]`
- `**`
	- Normally, `*` matches files and directories at the current level.
		- `**` makes it **recursive**, meaning it searches through all subdirectories.
		- matches 0 up to arbitrary many characters (including `/`)
	- only supported in bash 4.0 or up,
	- u might have to do this: `shopt -s globstar`
	- usage
		- echo * -> current files/folders (x subdirectories) 
		- `echo **` -> current files + directories + and all subdirectories & files
		- `echo **/*` ->  current files + subdirectory & files 
		- `echo */**` -> subdirectory + files (and not current files)
			![[Pasted image 20250313151233.png]]
#### Be careful with globbing
- If we use this in a wrong way it can lead to permanent damage
- bash doesn't differentiate between a folder and a parameter
	- name of a file can be interpreted as a parameter
- example
	- we have a folder named `-rf` lol
		- if we execute `rm *`, then `*` will expand so `-rf` will appear in the command -> `rm -rf`
		- then `rm` will think `-rf` is a parameter
	- `touch -rf`
		- we wanted to create a file named `-rf`, but `touch` processed it as multiple arguments 
		- `-r` (unknown option, so ignored) +  `f` (as filename, but it doesn't exist)
		- use `touch ./rf`
	- if we don't want folder `-rf` to be interpreted as a command option, use `./` in front
		- `rm ./*`

