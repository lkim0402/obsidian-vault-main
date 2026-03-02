
```bash
# 1. 원본 저장소를 upstream으로 등록
$ git remote add upstream <https://github.com/codeit-bootcamp-spring/awesome-food-spots.git>

# 2. 원본 저장소의 변경사항 가져오기
$ git fetch upstream

# 3. 내 main 브랜치에 병합 (또는 rebase)
$ git checkout main
$ git merge upstream/main

# 4. 내 GitHub 저장소에도 반영
$ git push origin main
```

> 🔁 이 과정을 정기적으로 수행하면 Fork 저장소가 원본과 계속 동기화되어 충돌을 줄일 수 있습니다.

- upstream
	- Original repo: `upstream` → the source repo you forked from
	- Your fork: `origin` → your GitHub repo (not the local)

# terminology

| location   | what it is                        | example                                                                |
| ---------- | --------------------------------- | ---------------------------------------------------------------------- |
| local repo | your local copy on your computer  | `~/awesome-food-spots`                                                 |
| origin     | Your fork on GitHub               | `https://github.com/**yourname**/awesome-food-spots.git`               |
| upstream   | The original repo you forked from | `https://github.com/**codeit-bootcamp-spring**/awesome-food-spots.git` |

# Flow explained

| Step | Command                       | What it does                                        |
| ---- | ----------------------------- | --------------------------------------------------- |
| 1    | `git remote add upstream ...` | Registers the original repo as `upstream`           |
| 2    | `git fetch upstream`          | Gets latest changes from the original repo          |
| 3    | git `checkout main`           | Switches to your local `main` branch                |
| 4    | `git merge upstream/main`     | Merges upstream changes into your fork’s local main |
| 5    | `git push origin main`        | Pushes updated `main`back to your GitHub fork       |
- You need to **`checkout` (switch to)** the branch you want to update **before** merging into it.
	- `git merge upstream/main` will merge `upstream/main` into your current branch
	- `upstream` refers to the upstream we added -> `upstream/main` = the `main` branch on the original repository you forked from

✅ To check your remotes:
`git remote -v`
You should see something like:

```bash
origin    git@github.com:yourusername/awesome-food-spots.git (fetch)
origin    git@github.com:yourusername/awesome-food-spots.git (push)
upstream  https://github.com/codeit-bootcamp-spring/awesome-food-spots.git (fetch)
upstream  https://github.com/codeit-bootcamp-spring/awesome-food-spots.git (push)
```


