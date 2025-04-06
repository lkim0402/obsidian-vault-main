- In our terminal, we're always in a directory
	- this directory is called a working directory (the folder that the shell is currently operating at)
	- commands can access this folder and act relative to this directory

### Absolute vs relative paths
- Absolute
	- starts with `/`
		- ~ counts as absolute because bash under the shell rewrites this as /home/user/
	- defines the complete path to a file
	- works anywhere (no matter our current working directory)
- Relative
	- being resolved according to our current working directory
	- `./Desktop, ../Desktop`
	- `.` indicates current directory