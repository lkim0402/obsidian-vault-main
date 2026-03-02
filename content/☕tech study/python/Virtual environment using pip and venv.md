- I'm only taking down notes for this because I want to stop googling the same docs over and over again & eventually just memorize this lol
- Resource
	- https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments
# `venv`
- Creates a "virtual" isolated Python installation, like a *disposable workshop* 
	- Allows you to manage separate package installations for different projects 
	- When you switch projects, you can create a new virtual environment isolated from other virtual environments
- DO NOT upload any `venv` folders to Github
	- Instead just upload a list of the packages your project needs

```bash
python3 -m venv .venv # (1)
source .venv/bin/activate # (2)
which python # (3)
deactivate # (4)
rm -rf .venv # (5)
```
1. `python3 -m venv .venv`
	- Create a new virtual environment in a local folder named `.venv`
	- `venv` will create a virtual Python installation in the `.venv` folder
2. `source .venv/bin/activate`
	- Activate the virtual environment before installing/using packages
	- this allows you to put the virtual environment-specific `python` and `pip` executables into your shell's `PATH`
	- While a virtual environment is activated, `pip` will install packages into that specific environment
3. `which python`
	- Confirm if the virtual environment is activated by checking the location of the python interpreter
	- If the virtual environment is active, the above command will output a filepath that includes the `.venv` directory by ending with the following `.venv/bin/python`
4. `deactivate`
	- Deactivate it (switching projects, leaving ur virtual environment etc)
- `rm -rf .venv`
	- Deleting the `venv` folder (common to delete them)
	- [[Bash commands]]
	- Make sure your `requirements.txt` file is up to date

📌`venv` vs [[Docker 🐳|docker]]
- `venv`
	- Primarily focuses only on isolating Python dependencies and versions for a specific project
- docker
	- Provides a containerization platform for packaging an *entire application and its dependencies* (including the operating system, libraries, and other software) into a self-contained unit called a container
	- Much stronger isolation than `venv`
- Basically (acc to this [Stack Overflow post](https://stackoverflow.com/questions/50974960/whats-the-difference-between-docker-and-python-virtualenv))
	- With a Python `venv`, you can easily switch between Python versions and dependencies, but you're stuck with your host OS.
	- With a Docker image, you can swap out the entire OS - install and run Python on Ubuntu, Debian, Alpine, even Windows Server Core.

📌Why explicitly state `python3` instead of `python` when creating `venv`?
- `venv` module is *explicitly a `python3` feature*, it doesn't exist on python 2
	- "Using the **Python 3** interpreter, find its built-in **`venv` module** and run it."
# `pip`
- The reference **Python package manager**, used to install and update packages into a virtual environment 
- If you `pip install` something in the `venv`, even if you `deactivate` and reactivate it again, all the installations & dependencies will remain

Examples of using `pip`
```bash
python3 -m pip install requests # (1)
python3 -m pip install 'requests==2.18.4' # (2)
python3 -m pip install --upgrade requests # (3)
python3 -m pip install -r requirements.txt # (4)
python3 -m pip freeze # (5)
python3 pip freeze > requirements.txt 

# Example of requirements.txt # (6)
requests==2.18.4
google-auth==1.1.0
```
- 📌`-m` flag
	- run a module as a script
	- `-m pip install ...` -> Use Python to run the `pip` module as a program
1. `python3 -m pip install requests`
	- Installing the `Requests` library
2. `python3 -m pip install 'requests==2.18.4'`
	- Installing with a specific version
3. `python3 -m pip install --upgrade requests`
	- Upgrade packages in-place using the `--upgrade` flag (latest version & all of its dependencies)
4. `python3 -m pip install -r requirements.txt`
	- You can declare all dependencies in a **requirements file** instead of installing packages individually
	- Requirements file
		- files containing a list of items to be installed using `pip install`
5. `python3 -m pip freeze`
	- export a list of all installed packages and their versions
	- `pip freeze` command is useful for creating requirements files that can *re-create* the exact versions of packages installed in an environment
	- `python3 pip freeze > requirements.txt `
		- You can save all the current requirements and put them in a requirements file
		- [[Redirection - Manage Data Streams]]
6. `Requirements.txt` -> a requirements file
	- If you don't state a specific version, `pip` will install the latest stable version it can find in PyPI (Python Package Index)
		- This is actually not recommended (breaking changes) so just pin the version

