- Configuring credentials (you can always change this)
	- name: `git config --global user.name "Leejun Kim"`
		- check by `git config user.name`
	- email: `git config --global user.email "(email)"
- Download GitKraken GUI

```bash
# global config
git init
git config --global user.name "lkim0402" 
git config --global user.email "estherleejunkim@gmail.com"

# config specific for a project 
git config user.name "개별 프로젝트용 이름" 
git config user.email "project@example.com"
```

- `git init`
	- Initializes new git repo. 
		- Creates a dir called .git that contains all git history
	- Top down: any child dirs will get tracked
	- DO NOT GIT INIT INSIDE A REPO (check w/ git status)