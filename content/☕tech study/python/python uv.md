- A [[python]] package and project manager written in Rust
- A drop in replacement for `pip`, `pip-tools`, `virtualenv` 
	- (python) [[Virtual environment using pip and venv]]
	- `uv` VS `pip virtualenv`
		- `pip` only handles package management and `virtualenv` handles only environment creation -> `uv` combines both functionalities in a single tool
		- `uv` -> often 10~100x faster, uses significantly less memory, better conflict resolution
	- [[Virtual environment]]
- What it does
	- Manages `pyproject.toml`, lockfiles (`uv.lock`), and project-specific virtual environments automatically.
	- Downloads and installs Python interpreters on demand
	- Installs and runs Python CLI tools in isolated environments
	- Uses global cache

# mac install & usage
```
brew install uv 
uv version
```

```
uv init explore-uv
cd explore-uv
tree -a
.
├── .gitignore
├── .python-version
├── README.md
├── hello.py
└── pyproject.toml
```
- [[git]] is automatically initialized and main git-related files like `.gitignore` + empty `README.md` are generated
- `.python-version`-> contains the Python version used for the proj
- `pyproject.toml` -> the main configuration file for project metadata and dependencies
	- if u wanna change the python version, check if version is ok in `pyproject.toml`, then go `.python-version` & change, then `uv sync`
	- then this creates a new virtual env in the proj directory -> `uv pip install -e`.
- `hello.py` -> simple file to help u get started quickly

Adding dependencies
```
uv add requests
uv add requests=2.1.2 
```
- `uv` combines the environment creation & dependency installation
	- the 1st time you run the `add` command, UV creates a new virtual environment in the current working directory and installs the specified dependencies
	- on subsequent runs, UR reuses the existing virtual env and only install/update the newly requested packages
- `add`
	- when u `add`, UV uses a modern dependency resolver that analyzes the entire dependency graph to find a compatible set of package versions that satisfy all requirements
	- automatically generates + updates `uv.lock` file
		- records exact versions of all dependencies & sub-dependencies
		- u can maintain `requirements.txt` too
			- `uv export -o requirements.txt` -> generates a `requirements.txt` from a uv lock file
	- updates the `pyproject.toml` 
```
uv remove scikit-learn
```

```
uv run hello.py
```
- `run` 
	- use when u wanna execute single file
	- `run` ensures that script is executed in the virtual env UV created

# `pip` to `uv` migration
```
pip freeze > requirements.txt
uv init.
uv pip install -r requirements.txt
```
# more on `pip` vs `uv`
- caching
	- `pip` -> caches downloaded wheel files + source distributions, but unpacks and copies the actual package files into each individual virtual environment (consuming more disk space and time)
	- `uv`-> uses global cache mechanism - if u install the same version of a package in multiple virtual envs on the same machine, `uv` links to the cached version rather than copying the files over and over
- tool consolidation
	- `pip & virtualenv` -> multistep ->  1st make `virtualenv` then use `pip` to manage packages
	- `uv` -> single unified tool -> use the `uv` CLI to create envs and manage packages
# sources
- [(Datacamp) Python UV: The ultimate guide to the fastest python package manager](https://www.datacamp.com/tutorial/python-uv)
- [official docs](https://docs.astral.sh/uv/)