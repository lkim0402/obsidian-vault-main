
>[!Overview]
>Directly writing constants into our code is referred to as using **magic numbers**.
>- We try to avoid this lol
- [[C lang]]
# Example
- Bad - wtf is 52?
```c
for (int i = 0; i < 52; i++) {
 // deal card
}
```
- Better, but introduces another problem
	- What if some other function manipulates this number?
```c
int deck_size = 52;
for (int i = 0; i < deck_size; i++) {
 // deal card
}
```
- Best
	- C has a preprocessor directive (also called a **macro**) for creating symbolic constants
	- Use **preprocessor directives!**
```c
#define DECK_SIZE 52

... // some code

for (int i = 0; i < DECK_SIZE; i++) {
 // deal card
}
```

# Preprocessor Directives
```c
#define NAME REPLACEMENT
#define PI 3.14159265
```
- At the time your program is *compiled*, `#define` goes through your code and replaces `NAME` with `REPLACEMENT`
- if `#include` is similar to copy/paste, then `#define` is analogous to find/replace
- NO semicolon!
- Convention
	- All caps  to make it clear that it's a constant and not a variable