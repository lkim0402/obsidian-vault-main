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
- Pipe diagram
	![[Pasted image 20250316135827.png]]
# Examples
#### Example 1
- `ls | wc -l`
- ls prints the files to `stdout`, then the pipe redirects the `stdout` is redirected to the `stdin` to the 2nd program `wc -l`. Then the result will be printed
- `ls | cat`
	- `wc -l`  and `cat` can also read from `stdin`
		- related:  [[Redirection - Manage Data Streams#What about `stdin`|What about stdin]]
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
# Common programs to use with `|`
#### `tee`
- With `|` and the `tee` command, you can create a `stdout` and write it to a file at the SAME time!
- Example
	- `echo 'Hello Bash' | tee hello.txt`
		- You want `Hello Bash` to be printed to `stdout` and also written into `hello.txt`
	- `echo 'Hello Bash' | tee hello.txt | wc -c`
		- multiple pipes
		- counting the text
- Appending
	- `echo 'Hello Bash' | tee -a hello.txt`
#### `ping`
- Allows us to send a ping packet to another server (ex. a remote server).
	- Related: [[Backend Explained#Server|Backend Explained - Servers]]
- We can use to test if our connection is reachable or disrupted
- Example
	- `ping google.com`
		- sends a ping packet to `google.com`
	- `ping non_existing.com 2>&1 | tee ping.txt`
		- Redirects `stderr (2)` to `stdout (1)`, so both error messages and normal output are now part of `stdout`.
		- The `tee` command takes `stdin` and writes it to both terminal (`stdout`) and `ping.txt`



