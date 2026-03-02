- Related: [[Connecting local git repo to server]]
# Adding a new repo
[[Configuring Git|Make a git repo first]].
```bash
#Staging  files
git add .
git add file1 file2

#Make a commit with all changes in the staged file
#Can group **similar changes together**
git commit -m "Initial commit"
git commit #opens up vim/text editor to put commit msg

# -a: adds every modified/deleted files (w/o `git add` command)
# -m: commit msg
git commit -a -m "New changes" #only works with files already tracked by git
git commit -am "New changes"

```
- See [[Git workflow (add and commit)]]
# git `status` and `log` (useful)
```bash
# MOST IMPORTANT GIT COMMAND!!
# Tells you which branch you're on and its relationship to the remote repo (ahead x commits, behind y commits, etc)
git status
```
- `git status`
	- 🟥 : Changes in your **working directory** (unstaged)
		- Changes not staged for commit
	- 🟩 : Changes that are in the staging area (staged)
		- Changes to be committed
```sh
# lists the commit history of your current branch for a given repo
git log
git log --oneline # shortens git log output
git log -p
```
# "Amending" just the previous commit
```bash
# If you forgot to add a file and commited, then add the file first and do the command
git add forgotten_file.py
git commit --amend 
#this allows to change commit msg
```
- Use this if you have to:
	- Add forgotten files
	- Change the commit message
# Reversing previous work
## Restore
```sh
# restoring
echo "Hello Git World" > hello.txt
git add hello.txt # in staging area
echo "Bye Git World" > hello.txt #overwriting
git restore hello.txt
```
- Restores `hello.txt` in the **working directory** to match the **staged** version (index)
	- the thing in staging overwrites local directory
	- doesn't change what's in the staging area
- doesn't require a commit

## Unstage
```sh
git restore --staged hello.txt
```
- unstages a file — it removes the file from the staging area (also called the index) and puts it back into the working directory without deleting or changing the file's content.

# Branches
```sh
# lists all branches in your local directory
git branch
git branch -v #more info

# creates branch **based upon the current HEAD** 
# does not switch
git branch test_branch

# switches branch
git switch test_branch
git checkout test_branch #checkout can do a million other things

# Create and switch to a new branch
git switch -c test_branch
git checkout -b test_branch

# Delete branch
# you have to be OUT of that branch & have it alr merged into another branch 
git branch -d text_branch
# deleting by force if you haven't merged it
git branch -D text_branch 

# Rename branch (we change it ON the branch)
git branch -m text2_branch
git branch -move text2_branch
git branch -M text_2branch # forcefully renames, overwritting the old one if any

# Merging branch
git switch master #change to receiver branch
git merge bugfix #merge into master
```
- Merging
	- [[Branches#Merging branches|Merging branches]] reference
# git `stash`
```sh
"""The most common commands"""
# stashes all uncommited changes(staged and unstaged)
git stash 
git stash save # same as above

# Removes the most recently stashed changes in your stash and re-apply them to your working copy
git stash pop

""" ====== Rare commands ====== """
#  Applies whatever is stashed away, without removing it from the stash. # This can be useful if you want to apply stashed changes to multiple branches
git stash apply

"""multiple stashes (which is kinda unusual..lol)"""
git stash list
git stash apply stash@{1}
git stash pop #pops the last stash
git stash drop stash@{1} # deletes the stash (if you aren't using pop)
git stash clear # empties stash list
```
- Explained more here: [[Git stash]]
# git `diff`
> lists all the changes in working directory NOT staged for the next commit
- shows the "differences" in the content in working and staging directory
	- 아직 `git add`하지 않은 차이점
- [[Git diff]]
```sh
git diff 
# shows what you could have added (staged) but haven't added yet
# a - staging area, b - working directory

# git (last commited state) vs staging area (added, but uncommited)
git diff --staged
git diff --cached
# shows what you've staged and are about to commit

# lists all changes in working tree since last commit
git diff HEAD

# Diff-ing specific files
git diff HEAD [filename1] [filename2]
git diff --staged [filename]

# Comparing changes between tips of branch1 and branch2
# changes between ALL commited files that exist in those 2 branches!!
git diff branch1..branch2
git diff branch1 branch2

# Diff. bet. commits (use the commit hashes)
git diff commit1..commit2
```
- A more comprehensive explanation: [[Git diff]]
# git `tag`
- [[Git Tags]]
```sh
# check all tags
git tag

# create lightweight tag in current commit
git tag [tagname]

# create annotated tag
git tag -a [tagname] -m "message"

# get tag's commit info
git show [tagname]

# go to tag's code
git checkout [tagname]
```
# Removing all existing git history
```sh
rm -rf .git
```

# Working with remote git repo 
## Cloning a repo
```sh
git clone <url> #clones the entire repo
git clone -b branch1 <url> #clones a specific branch1
```
- `git clone <url>`
	- not tied to github but its a git command
	- Works with any git repos 
- A remote tracking branch: `<remote>/<default-branch>`
	![[Pasted image 20241007235207.png|500]]
	- In my computer
		- `master` branch: When you clone a repo, whatever the repo has it's default branch, that's what we will start with (in this case `master`).
		- Remote tracking branch `origin/master`: Points to the `master` branch in the `origin` remote!
			- `master` because it's the default branch
```sh
# we can see the remote branches our local repo knows about
git branch -r

# origin/HEAD -> origin/master  # HEAD points to the default branch
# origin/master  # Remote branch tracking the remote's master branch
# origin/feature  # Other remote branches (e.g., a feature branch)

```
	
## Push an existing repo from the CLI
```
git remote add origin https://github/com/lkim0402/(repo)
git branch -M main
git push -u origin main
```

- [[Connecting local git repo to server]]
- [[Git Fetch VS pull]]


