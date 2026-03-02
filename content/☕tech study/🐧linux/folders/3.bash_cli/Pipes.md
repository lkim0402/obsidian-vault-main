
>[!What is a pipe]
>- A mechanism that redirects the **standard output (`stdout`)** of one command into the **standard input (`stdin`)** of another command
>- So you can chain multiple commands together and build more complex functionalities
>- General syntax: `command | command | .... | command`
- Motivation
	- Without pipes, how do we count the number of files in a directory?
	- `ls > output.txt`, `wc -l output.txt`, `rm output.txt`
	- the `output.txt` should be in a different folder because `ls` will count it too
	- Use pipes, it makes your life easier by allowing you to combine multiple programs together
	- no need for temporary file
- Basically after the `|` there should be a *command*, and NOT A FILE
- Pipe diagram
	![[Pasted image 20250316135827.png]]
# Examples
- [[Bash commands]]
- [[Bash commands#Commands used with pipe ` `|Bash commands - Commands used with pipe]]
#### Example 1
- `ls | wc -l`
	- ls prints the files to `stdout`, then the pipe redirects the `stdout` is redirected to the `stdin` to the 2nd program `wc -l`
	- Then the result will be printed
- `ls | cat`
	- `wc -l`  and `cat` can also read from `stdin`
		- related:  [[Redirection - Manage Data Streams#What about `stdin`|What about stdin]]
- `ls -l | tail -n3`
	- show the last 3 lines of `ls -l`
- `ls -1 | tail -n8 > test/ls.txt`
	- get the last 8 lines from `ls -1` and put that on `test/ls.txt`
#### Example 2
```bash
leejun@leejun-VirtualBox:~/Desktop$ du -h text.txt not-exist.txt 2>&1 >/dev/null
du: cannot access 'not-exist.txt': No such file or directory

leejun@leejun-VirtualBox:~/Desktop$ du -h text.txt not-exist.txt 2>&1 > /dev/null | wc -l
1
```
- 1st command - redirects error to current `stdout` (terminal) then redirects `stdout` to `/dev/null`
- 2nd command - you can use this to count the number of errors
	- `> /dev/null` → Discards `stdout`, but `stderr` is still printed to the terminal (as it was redirected to previous current `stdout` (terminal)).
	- the pipe receives this `stderr` input for `wc -l`
	- You're not piping from `/dev/null`—you redirected only `stdout` there. The remaining `stderr` output is still available for piping.

