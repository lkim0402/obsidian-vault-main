- `.gitignore`
- Ignores files you NEVER want to commit!
	- API keys, credentials, and other application secrets
	- Log files
	- Operating system files
	- dependencies and packages
	- You put these in a separate file
- Create `.gitignore` file at the root of a repo
	- Files: `test.py`
	- Ignoring folders & everything in it: `folderName/`
	- Extensions (wildcards): `*.log`
	- NOT ignoring: `!important.log`
- Usually there's templates alr prepared
- Creating a useful `.gitignore` file
	- https://www.toptal.com/developers/gitignore

```sh
echo "*.log" > .gitignore
echo "node_modules/" >> .gitignore
echo "!keep.log" >> .gitignore
```
- you can just easily add stuff like this