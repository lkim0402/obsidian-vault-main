- Both are getting changes down from a remote repo
- If you plan on collaborating, these are critical

# git `fetch`
```sh
git fetch origin
```
- Downloads commits, files, and refs (branches, tags) **from a remote repository** to your local repo.
- Updates your **remote tracking branches** (like `origin/main`), but **does NOT change your current working branch** or working files.
- Allows you to see what’s new on the remote without merging or modifying your current branch
- After fetching
	- you can check the changes (e.g., `git log origin/main`) before deciding what to do next
# git `pull`

```sh
git pull origin main
```
- Combines `git fetch` + `git merge` (by default).
- Downloads changes from remote and **immediately merges them into your current branch**.
- Updates your files directly, so your current branch reflects the latest remote commits after the pull.
- There might be a conflict