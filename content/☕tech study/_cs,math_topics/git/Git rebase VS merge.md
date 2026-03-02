# merge
- Non-destructive integration
- It preserves the original commit history of both branches.
- A new merge commit is created
![[merge-fastforward.png]]![[merge-nonfastforward.png]]
# rebase
- Makes a new "base"
- Linearizes history. git rebase integrates changes by moving or combining a sequence of commits to a new base commit. It rewrites commit history.
- **Rule of Thumb: NEVER rebase a public/shared branch.**
#### `main` 기준
![[rebase-main.png]]
```bash
git checkout ui-feature
git rebase main
```
#### `ui-feature` 기준
![[rebase-ui-feature.png]]
```bash
git checkout main
git rebase ui-feature
```
