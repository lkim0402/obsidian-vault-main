- At the library, program, or function level, use comments to describe _what_.
- Inside the library, program, or function, use comments to describe _how_.
- At the statement level, use comments to describe _why_.
### Proper use of comments
> Assume that the reader of comments has no idea what the code does.. Don't assume you'll remember why you made specific choices

1. For a given library, program, or function, comments are best used to describe what the library, program, or function does
```C++
// This program calculates the student's final grade based on their test and homework scores.
```
2. Reminding yourself or others why the problem is solved in that way
```cpp
// We decided to use a linked list instead of an array because
// arrays do insertion too slowly.
```
3. To just... remind you what the code does!
### Bad vs good comments
- You could describe why the code is doing something instead of the what
```cpp
//bad
//set sight range to 0
sight = 0

//good
// the player drank a potion of blindness and cannot see anything
sight = 0
```

### Two types
```c++
// single line comment

/*
multi-line comment
*/
```