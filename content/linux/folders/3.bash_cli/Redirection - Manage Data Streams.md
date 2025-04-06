> [!Streams]
> - A flow of data between a program and its input/output
> - you can redirect the output of a program into a file
## Standard Streams: `stdin`, `stdout`, `stderr`
- It's how a command can interact with the terminal
- By default, there are 3 communication channels for data
	- file descriptors
		- is a number that represents an input/output (I/O) stream
		- Linux assigns numbers 0-2 to these streams
	- diagram
		![[Pasted image 20250315130833.png]]
	- `0: stdin`: standard input (keyboard)
		- If the program wants to access our keyboard, then it could start listening to the `stdin`, then the program could receive input for our keyboard
	- `1: stdout`: standard output (screen)
		- The program wants to display something on the screen
		- So everything our program outputs, it just sends it to the stream `stdout`, then our terminal displays that in our terminal
	- `2: stderr`: standard error 
		- when terminal wants to output errors
		- by default this also gets printed to the screen
## Redirection operators
#### Redirecting `stdout`
- These operators only **redirect** `stdout` to a file (doesn't capture`stderr`)
	- By default, `stdout`(standard output) is printed to the screen.
	- Redirection tells [[Bash, shell, terminal, command line, kernel|bash]] to send`stdout` to a file instead.
	- Adding/removing 1 to the operator doesn't matter
- Operators
	- `>`
		- Redirects an output directly into a file
		- Redirects only `stdout` by default (doesn't capture `stderr`)
		- overwrites file
		- `echo 'hello' > test.txt`
			- Creates new file if file doesn't exist
			- same with `echo 'hello' 1> test.txt`
	- >>
		- Redirects stdout (appends to file)
#### Redirecting `stderr`
- Some reasons to use this 
	- We may want to use this when we want to ignore errors (a program may print errors)
	- We are only interested in the errors and want to redirect them into a file (to have a look at them at a later date)
- Operators
	- If there are no errors, the file is emptied (for both operators)
	- `2>`
	- `2>>`
#### Redirecting `stdin` AND `stderr`
- Just use them in one line 
	- `du -h test.txt not_existing.txt > output.txt 2> error.txt`
- You can ignore the errors using `/dev/null` 
	- Redirect to a null device, a file we can open and write into it and it will be discarded by the [[Operating systems (OS)|operating system]] -> `/dev/null`
#### Redirecting `stderr` to `stdout`
- It easily allows us to store both outputs in the same file
- `&1` stands for *current* `stdout`
	- `[command] 2>&1`
		- we take our `stderr` and redirect it to the current `stdout` 
			- both messages (`stdout, stderr`) will be combined into the same output stream (which could be saved to a file or displayed on the screen)
		- `du -h test.txt not_existing.txt 2>&1
	- `[command] > out.txt 2>&1`
		- we take the `stdout` from the `[command]` and the `stderr` to the out.txt
		- `stdout` is first redirected to `out.txt`, then `stderr` is redirected to the same destination as `stdout` (in this case `out.txt)
		- `stdout` should be redirected first when you're combining redirections for both `stdout` and `stderr`
- Changing orders (order of redirects is important!)
	- declare the mappings before they are executed
	- Correct: `du -h text.txt not_existing.txt > out.txt 2>&1`
	- Wrong: `du -h text.txt not_existing.txt 2>&1 > out.txt`
		- error is shown in terminal, then `out.txt` will contain only the normal output
		- `stderr` redirected to current `stdout` -> terminal
		- then `stdout` is redirected to `out.txt`
- Using `&>`
	- `du -h test.txt not_existing.txt &> output.txt`

#### What about `stdin`
- some programs also accept user input
- For example ([[Basic Bash commands|bash commands]])
	- `wc -l` lets us access our `stdin` (can use ctrl + d to quit)
	- `cat`
- can we redirect a file into `stdin`?
	- `wc -l < output.txt`
	- Bash will read `output.txt` and use this as `stdin`
- combining example
	- `wc -l < output.txt > text.txt`
## Chart

| Operator | What It Captures | Overwrites/Appends? |
| -------- | ---------------- | ------------------- |
| `>`      | stdout (1)       | Overwrites the file |
| `>>`     | stdout (1)       | Appends to the file |
| `2>`     | stderr (2)       | Overwrites the file |
| `2>>`    | stderr (2)       | Appends to the file |
| `&>`     | stdout + stderr  | Overwrites the file |
| `&>>`    | stdout + stderr  | Appends to the file |

