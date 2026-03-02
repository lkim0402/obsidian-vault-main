# The Basic Git Workflow
![[Pasted image 20240916223302.png]]
- `git add .`
	- select which changes to stage before committing
	- "include these in our next commit"
	- group similar changes together
- `git commit`
	- commit changes from the staging area
	- git expects a message! (summarizes change)
	- when we make a commit, we're making an update to the `.git` folder
	- Each commit has a unique **hash**
		![[Pasted image 20240918172726.png]]
		- Each commit also references at least one parent commit that came before
## How it works
![[Pasted image 20240916222800.png]]
#### Working directory
- The folder I'm currently working in
#### Staging area 
- Where you "stage" changes by using `git add`. 
- A middle step where changes are prepared before committing.
- NOT a temporary storage that gets cleared after a commit. Rather, it's a **snapshot of what will go into your next commit**
- The staging area **always exists and reflects the content of your last commit by default**
- When you stage changes (using `git add`), you're updating this snapshot.
#### Git repository
> A "repo" is a **git workspace** which tracks and manages files within a folder.
- the `.git` folder
	- The state of your files after the last commit, which is stored in your Git repository (Git history).
- We should create a new git repository anytime we want to use Git with a project, app, etc 
- We can have as many repos on our machine!
- Every git repo has its own history/contents
- If you want to use git, go in each project folder and create a git repo
	- `git init`