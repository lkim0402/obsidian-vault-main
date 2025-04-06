- Not mandatory, but convenient!
- What happens if I have **uncommitted changes** on one [[Branches|branch]] and I switch to another?
	1. They follow along when you switch
	2. git won't let you switch if there is a conflict between files
- `git stash`
	- allows you to switch branches or pull the latest changes from the remote repository without losing your uncommitted work
	- You can then apply these stashed changes back to your working directory whenever you're ready
- Where the stashed changes are:
	- The changes are removed from your working directory but are stored safely in that stash stack, which is global to your repository, not tied to a specific branch.
### Example
![[Pasted image 20241001210144.png|500]]
![[Pasted image 20241001210156.png|500]]
After doing some work on another branch, we return to our branch.
What can we do now:
1. `git stash pop`
	![[Pasted image 20241001210226.png|500]]
2. `git stash apply`
	![[Pasted image 20241001210429.png|500]]

### Multiple stashes
What if you make a new stack: (but ppl don't really use it lol)
- Every time you run `git stash`, Git creates a new stash entry and pushes it onto the stash stack.
- Each stash entry is assigned a unique identifier, like `stash@{0}`, `stash@{1}`, etc., where `stash@{0}` is the most recent stash.
- You can see this with `git stash list`
	- `git stash apply stash@{1}`