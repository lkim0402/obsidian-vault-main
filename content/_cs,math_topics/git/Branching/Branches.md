> - Alternative timelines for a project 
> - A lightweight movable **pointer to a specific commit**
- Linear vs simultaneous
	- Git commit has a linear workflow where each commit has a parent commit ([[Git workflow (add and commit)|reference]])
	- But in real world projects people work **simultaneously** (fix bug, new feature, etc)
- While it might seem like you're working with a copy of your repository when you switch branches, what's really happening is that Git is changing which commit your HEAD is pointing to and updating your working directory to match that commit.
- [[Git Commands#Branches|Branches commands]]
- If I'm on a branch and I have some work that's not committed, when I switch to a different branch it will be lost
	- You either need to `commit` or `stash` them
	- This is because you worked with a conflicting file
- If I'm on a branch and I created a new file (meaning you haven't committed anywhere), when I switch **the file will come along with me**. 
	- It will just follow you to every branch you go to until you commit it.
## Master vs Main Branch
- default branch
	- the main branch that GitHub shows first when you visit a repository
- `master`
	- In git, we are always working on a branch. The default branch name is **master**.
	- It's just like any other branch in git's perspective, but some ppl treat it as their "official working codebase", but that's only a preference
	- You can rename it
- `main`
	- In 2020, **Github** renamed the default branch to **main**.
	- The default **Git** branch is still **master**.
	- If you initialize the git repo in github with a file (like a README file), then it will have `main` as the default branch (which you can change in settings)
	- More and more people are using ` ` more now
## HEAD -> master
![[Pasted image 20240918174235.png]]
`HEAD`
- A pointer that points to the branch currently active in my [[Git repository|git repo]]. 
	- So HEAD -> `master`: HEAD currently looking at the latest commit in `master` branch
- Like bookmarks, you can switch between branches
- When you make a new commit on the current branch, the branch pointer moves forward to the new commit, and HEAD moves along with it (since it's pointing to the branch).

- 2 commits on `master` branch
	![[Pasted image 20240918201330.png|400]]
- Made new branch `DarkMode` and **switched**
	![[Pasted image 20240918201437.png|370]]
- Commited on the new branch `DarkMode`. It diverges when you actually make a change and commit.
	![[Pasted image 20240918201533.png|400]]
	![[Pasted image 20240918202613.png]]
	- (Ignore branch name `oldies`, doesn't matter)
	- master is like a checkpoint before the new changes were made. HEAD now points to `oldies` with the new branch


## Merging branches
- Merging
	- We use it to combine changes from one branch into one another!
	- A common workflow is we leave the 'source of truth' (usually master), make a branch based off that main branch, and merge back into it (if everyone likes it)
	- Two important concepts:
		- We merge branches, not specific commits
		- We always merge to the current HEAD branch
- Steps
	1. Switch to (or checkout) to the branch you want to merge the changes into (the receiving branch)
	2. Use `git merge` command to merge branches from a specific branch **into** the current branch
```
git switch master
git merge new-feature
```

|                                         | New Commit? | Conflicts? |
| --------------------------------------- | ----------- | ---------- |
| Fast forward merge                      | No          | No         |
| Non-Fast-Forward Merge (No Conflicts)   | Yes         | No         |
| Non-Fast-Forward Merge (With Conflicts) | Yes         | Yes        |

#### Fast forward merge
- `master` just "fast forwarded" a couple of commits; there wasn't additional work on the `master` branch
- We just move the pointer forward, so we don't need a new commit
	![[Pasted image 20240920230902.png]]
	![[Pasted image 20240920230924.png]]
	![[Pasted image 20240920230940.png]]
#### Non-fast forward merges: More commits on master
- both branches have new commits since they diverged
![[Pasted image 20240921141932.png]]
![[Pasted image 20240921141952.png]]
- we end up with a **merge commit**
	- git makes a commit for us on the branch we're merging into
		- It will always ask for a commit message!
	- the new commit has now 2 parents!
- gitkraken (I deleted the middle branch)
	![[Pasted image 20240921144826.png]]
#### Resolving Merge Conflicts
- Depending on the specific changes, git may not be able to automatically merge -> merge conflicts (**manually resolve**)
	- `git` tells us that there's conflicts
	- Open the file(s) w/ conflicts
	- Decide which branch's content you want to keep in each conflict (or keep both)
	- Remove the conflict "markers"
	- Commit the changes
- Sample
	![[Pasted image 20240921162617.png]]
	- I merged `test1` branch into `combo` branch (new branch from `test2`) 
- The conflicts are **decorated** w/ markers
	![[Pasted image 20240921150239.png]]
	- `HEAD`: The content from your current branch
	- Under `=====`: The content from the branch you are trying to merge