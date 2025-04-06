### Case 1: Existing repo
> There is an existing [[Git repository|git repo]] on your local machine
- Create a new repo on Github (go to the website and do it manually!)
- Connect your local repo (add a remote to your local machine)
```sh
# Checking if any existing repos exist in your repo
git remote
git remote -v

# Adding a new remote (somewhat common to have multiple remotes)
# the name origin is just a conventional name
git remote add <name> <url>
git remote add origin https://github.com/yourusername/your-repo-name.git

# optional
git remote rename <old><new>
git remote remove <name> 

# Pushing to remote
git push <remote> <branch>
# if master doesn't exist, it will create master branch
# if it exists it will just push to the master branch
# Pushing master branch to github (you don't need to be in the branch)
git push origin master 
git branch -M main # it's convenient to rename your branch to main before pushing

# Once you’ve pushed once with the -u (upstream) flag, future pushes from that branch will automatically go to the same branch on the remote without needing to specify origin master
# the upstream tracks the local branch on the origin repo
git push -u origin main  
git push # you have to be on the branch

```
- Push up your changes to Github
	- You can rename your `master` branch to `main` and then push
	- Related: [[Branches#Master vs Main Branch|Master vs Main Branch]]
- upstream tracking
	- When you use the -u flag (or --set-upstream), you are telling Git to set up an upstream tracking relationship between your local main branch and the main branch on GitHub (origin/main).
	- After this, Git remembers that when you're on the local main branch, it should push to origin/main by default.

More about pushing
```sh
# pushing our local pancake branch up to a remote branch called waffle
git push <remote name> <local branch>:<remote branch>
git push origin pancake:waffle
git push -u origin pancake:waffle # possible, but not recommended
```

### Case 2: Starting from scratch
- Create a brand new repo on github
- Clone it down to your local machine
- Do some work locally
- Push up your changes to github