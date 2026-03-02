# while
- Use when you want a loop to repeat an unknown number of times & *possibly not at all*
```c
while (condition) {
    // code to be executed...
}
```
# do while
- Use when you want a loop to repeat an unknown number of times but *at least once*
	- like prompting user for input
```c
do {
    // code to be executed...
} while (condition); // <-- Don't forget the semicolon!
```
# for loop
```c
for (start; expr; increment) {

}
```
- steps
	- `start` is executed
	- `expr` is checked
		- if it is `true`, the body of the loop executes
			- Then, the `increment` is executed
		- if it is `false`, the body of the loop does NOT execute and the for loop finishes
- Use when you want a loop to repeat a discrete number of times, though you many not know the number at the moment the program is compiled

