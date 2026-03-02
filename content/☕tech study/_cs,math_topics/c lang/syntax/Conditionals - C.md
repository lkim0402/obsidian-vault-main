# If else
- Example 1
```c
if (x > y) {
	printf("x is greater than y\n");
}
if (x < y) {
	printf("y is greater than x\n");
}
if (x == y) {
	printf("x and y are equal\n");
}
```
- Why is this badly designed?
	- The boundaries are not set properly, giving the computer more things to do so it makes the operations slower!
	- The questions are going to be asked no matter what, wasting the computer's energy
- Example 2
	- This is better! The cases are mutually exclusive!
```c
if (x > y) {
	printf("x is greater than y\n");
} else if (x < y) {
	printf("y is greater than x\n");
} else {
	printf("x and y are equal\n");
}
```
# Ternary
- basically a tighter version of if else, if you're making simple decisions
```c
int x = (expr) ? 10 : 20;
```
# `switch()`
- It is a conditional statement that permits enumeration of discrete cases, instead of relying on boolean expressions
- It's important to `break;` between each case, or you will "fall through" each case (unless that's expected)
```c
int x = GetInt();
switch(x)
{
	case 1:
		printf("1!\n");
		break;
	case 2:
		printf("2!\n");
		break;
	case 3:
		printf("3!\n");
		break;
	default:
		printf("Sorry!\n");
}
```