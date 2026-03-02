The question: Is it possible to create a problem that determines, for any given program and its input, whether that program will halt or loop?

- Let's say there is machine `H` that correctly predicts if it will get stuck for any given program.
	- `H(p1, 5) -> loop`
	- `H(p2, 10) -> halt`
- Let's say there is a machine `N` that does the opposite of the given input of `halt`/`loop`
	- `N(halt) -> loop`
	- `N(loop) -> halt`
- Now, let's combine these programs and call it `X`
	- A machine `P` that duplicates whatever is inputted
	- `H` that will receive 2 arguments (program, input)
	- `N` that will negate `halt`/`loop`
- Let's try inputting `X` into `X`
	- `P` -> duplicates `X`
	- `H` -> `H(X,X)` 
		- This will give us an output, let's say `halt`
	- `N(halt) -> loop` 
		- Now `halt` is negated, resulting in `X` in a `loop`
	- This is a contradiction because `H` said `X` will `halt`, but it's in a `loop`
- Therefore, `H` cannot exist