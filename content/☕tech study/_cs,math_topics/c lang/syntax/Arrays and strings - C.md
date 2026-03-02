
>[!arrays]
>A sequence of values back to back (contiguous) in memory all of which is the same data type
- A [[Terminology - ADT, Data Structure, Implementation, Algorithm#Data structure|data structure]]
# Declaration
- `type name[size];`
	- Telling the compiler that this is an array of integers of size `size`
	- The elements in the array are all *contiguous* in memory
```c
# individual element syntax
int scores[3];
scores[0] = 72;
scores[1] = 73;
scores[2] = 33;

# instantiatino syntax
bool truthtable[3] = {false, true, true}
bool truthtable[] = {false, true, true} 
// size is optional in instantiatino syntax  
```

- In C, if you pass an array as input to a function, you  don't have to know in advance what the size is
- BUT so actually u need to pass in how big it is in the function too (`int length` in this example)
```c
const int length = 5;
int scores[3] = {1,2,3};
average(length, scores);

float average(int length, int array[]) { // pass in length as well
	int sum = 0;
	for (int i = 0; i < length; i++) {
		sum += array[i];
;	}
return sum / (float) length;
}
```
# String
```c
#include <stdio.h>

int main(void)
{
    char c1 = 'H';
    char c2 = 'I';
    char c3 = '!';

    printf("%i %i %i\n", c1, c2, c3);
}
```
- If you do this, then ASCII codes are being printed

- String is ***actually an array of chars***
```C
string s = "HI!";
printf("%c %c %c", s[0], s[1], s[2]); 
```
- use `string.h`

Other examples
```c
int main(void)
{
    string words[2];

    words[0] = "HI!";
    words[1] = "BYE!";

    printf("%s\n", words[0]);
    printf("%s\n", words[1]);
    // same with
	printf("%c%c%c\n", words[0][0], words[0][1], words[0][2]);
    printf("%c%c%c%c\n", words[1][0], words[1][1], words[1][2], words[1][3]);

}
```
# NUL
- the last byte of a string will always be 0, a special delimiting character *NUL*
```C
string s = "HI!";
printf("%i %i %i %i\n", s[0], s[1], s[2], s[3]); // 72 73 33 \0
```
- the variable indicate where the string begins, the`\0` indicate where the string ends
	- Every string's actual length is length + 1
- The double quotes imply that it's a string & it needs to be terminated with backslash `0`

```c
int main(void)
{
	string s = get_string("Input: ");
	printf("Output: ");
	// inititalize strlen here too!!
	for (int i = 0, n = strlen(s); i < n; i++)
	{
		printf("%c", s[i]);
	}
	print("\n");
}
```

📌Determining the `NUL` terminator
- **As a String:** Functions like `printf("%s")` or `strlen()` are specifically built to **stop** the moment they see the _first_ `\0`. To them, this `0` means "end of string".
- **As an Array:** When you use a `for` loop (e.g., `for (int i = 0; i < 10; i++)`), _you_ are in charge. You tell it the exact length, so it treats the `0` just like any other number and keeps going.
# Multi-dimension
- Arrays can consist of more than a single dimension
	- You can have as many size specifiers as you wish
- `bool battleship[10][10]`
	- 10 x 10 grid of cells
	- In memory though, it's just a 100-element 1D array
- Multi-dimensional arrays are great abstractions to help visualize gameboards or other complex representations
- We can't treat entire arrays themselves as variables
	- we CANNOT assign one array to another using the assignment operator (not legal C)
	- we must use a loop to copy over the elements one at a time