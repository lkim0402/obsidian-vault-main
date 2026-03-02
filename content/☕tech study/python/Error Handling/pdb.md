- To set breakpoints in our code we can use `pdb` by inserting this line
```python
import pdb; 
pdb.set_trace()

# Also commonly on one line
import pdb; pdb.set_trace()

# Common PDF commands
# l (list)
# n (next line)
# p (print)
# c (continue - finishes debugging (basically continue code))
# q (quit, w/ error msg)
# a (all variables)
```
- If you have variable names that conflict, it will just do that command (lol)
	- so if you want to print the variable, add `p`
	- ex) printing variable `c` with `pdb`: `p c`

Example
```python
first = "First"
second = "Second"
pdb.set_trace()
result = first + second
third = "Third"
result += third
print(result)
```
- How it looks
	![[Pasted image 20241007224216.png]]
- you can type in variables to see their values at that time
- list command looks like this
	![[Pasted image 20241007224520.png]]
- It's not something you keep using, so usually it's not with all the modules on top. You can define it within the function!