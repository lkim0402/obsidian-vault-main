
>[!tag]
>A tag is a **named reference** to a specific commit. Unlike branches, tags don’t move — they always point to the exact same commit.
>- can be placed on *any* commit
- **tags are not pushed automatically** — **even if the commit they point to is pushed**
- Advantages
	- Log the versions
	- Rollbacks
	- Documentations + release notes
- [[Git Commands#git `tag`|tag commands]]
#### Example
```sh
git commit -m "Add v1.0.0 changes"
git tag v1.0.0
git push origin main
```
- ✅ The commit is pushed to `main`.
- ❌ The `v1.0.0` tag is **NOT** pushed unless you do:
```sh
git push origin v1.0.0    # push one tag
# or
git push origin --tags    # push all local tags
```
# Types
#### Lightweight tag
```sh
# Adds the v1.0 tag in the current commit 
git tag v1.0
```
- Just points to the current commit (no metadata)
- `git checkout v1.0`
	- you can use this command to move to that point in code
#### Annotated tag
```sh
git tag -a v1.0.0 -m "Release version 1.0.0"
# you can also tag older commits
git tag -a v1.0.0 <commit-hash>
```
- has author, message, date (recommended)

# Branch & Tags differences

|           | Branch                                          | Tag                                                     |
| --------- | ----------------------------------------------- | ------------------------------------------------------- |
| Purpose   | A workspace for development                     | Used to mark and reference a specific point (version)   |
| Mobility  | Moves along with new commits (keeps updating)   | Fixed to a specific commit and does not change<br>      |
| When Used | For feature development, bug fixes, experiments | For deployments, releases, or when reaching a milestone |
| Type      | Pointer (a moving reference point)              | Label (a fixed reference point)                         |
# Sematic versioning

| Version format | Desc                                                                                            |
| -------------- | ----------------------------------------------------------------------------------------------- |
| `MAJOR`        | Major changes incompatible with previous versions (e.g., removing features, structural changes) |
| `MINOR`        | Adding new features compatible with previous versions (e.g., new APIs)                          |
| `PATCH`        | Bug fixes compatible with previous versions (e.g., fixing errors, optimizations)                |
#### Tag Management Strategy by Development Stage
Versions before official release can also be clearly managed using tags:

- `v1.0.0-alpha`: Early experimental version
- `v1.0.0-beta`: Beta testing version
- `v1.0.0-rc.1`: Release Candidate (candidate for final release)