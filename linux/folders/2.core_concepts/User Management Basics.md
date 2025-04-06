### Categories
- System accounts
	- Responsible for running background tasks on your system (ex. webserver, database, etc)
	- They don't have a home directory
- Regular users
	- They have access to their own files and directories
	- CANNOT perform administrative tasks or access other user's files w/o permission
	- we want situations where the regular uses will temporarily gain root privileges
		- [[Basic Bash commands#Sudo|sudo]] allows to do that
- Administrator (Sudo user)
	- Just added to the `sudo` group
	- Has `sudo` privileges but must still use `sudo` to execute privileged commands
- Superuser (root)
	- has UNRESTRICTED access to the entire system (including files in the home directories of regular users)
	- Can add/remove users, install software
	- can change the configuration of the system

### In Ubuntu
- By default, Ubuntu **does not set a password for root** during installation.
- This means you cannot log in directly as root from the login screen or via SSH.
	- Technically you can manually enable the root account by setting a password for root, but it's NOT  recommended (for security concerns)
- Instead, you use `sudo` to execute commands with root privileges, and this uses your **regular user's password**.