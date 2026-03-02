> You want each project to have their own environment. Virtual environments isolates Python projects by creating separate environments. Each environment has its own installation of Python packages, which avoids conflicts between different projects.

### Pip
> The standard package manager for Python, used to install, upgrade, and remove [[Python]] packages
- You basically download external modules using `pip`
	- As of Python 3.4, it comes installed in Python by default
- Uses `requirements.txt` for dependency management
- You need to handle virtual environments separately

Just python
```python
python3 -m pip install PACKAGE_NAME
```
With virtual environment
```bash
#Create virtual environment
python -m venv env

# Activate virtual environment  
source env/bin/activate

#Install requirements
pip install -r requirements.txt 

# Listing al installed packages
pip freeze
pip freeze > requirements.txt

# When using a virtual environment, `sys.executable` points to the Python executable within that virtual environment. This helps ensure that you are using the correct Python interpreter when running scripts or commands.
python
>>> import sys
>>> sys.executable #'use/bin/python'
```

### Pipenv
https://www.youtube.com/watch?v=6Qmnh5C4Pmo&t=1s
- 시간나면 볼것

> Manages both virtual environments and project dependencies in a more integrated way. It aims to provide a unified tool for dependency management and virtual environment creation.
- Automatically manages `Pipfile` and `Pipfile.lock` which replace `requirements.txt` for specifying and locking dependencies.
- Automatically creates and manages a virtual environment for your project, ensuring isolation of dependencies.
- `Pipenv` cheat sheet
	- https://gist.github.com/bradtraversy/c70a93d6536ed63786c434707b898d55
```bash
pip install pipenv

# Creating a new project and virtual environment, generating a Pipfile in the directory
pipenv install

# Activating the environment
pipenv shell

# Installing the Python package you want to install
# Updating 'Pipfile' and 'Pipfile.lock'
pipenv install package_name
```

### Pip vs Pipenv install
`pip install langchain`
- Installs the langchain package directly into the currently active Python environment, which could be either the global environment or a manually created virtual environment
`pipenv install langchain`
- Installs the `langchain` package into a virtual environment managed by `pipenv`. If no virtual environment exists, `pipenv` will create one for you.
	![[Pasted image 20240813183559.png]]