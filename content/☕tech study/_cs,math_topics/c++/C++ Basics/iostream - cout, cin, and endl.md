### `std::cout`

```cpp
#include <iostream>

int main()
{
	int x { 5 };
	std::cout << "Hello" << "World!" << std::endl; 
	std::cout << "x is " << x;
	std::cin.get()
}
```
- `std::cout`
	- prints to console
	- `cout` = character output stream
	- The output "gets in line", and is stored in a region of memory set aside to collect such requests - **buffer**
		- Periodically, the buffer is **flushed**, meaning all of the data collected in the buffer is transferred to its destination (in this case, the console)
	- So if your program crashes, aborts, or is paused (e.g. for debugging purposes) before the buffer is flushed, any output still waiting in the buffer will not be displayed.
- `std::endl`
	- end line
	- basically **newline**
- `<<`  
	- insertion operator
	- can be used to concatenate (link together) multiple pieces of output
	- think of them as functions!
- `std::cin.get()`
	- This will just wait for us to press enter

### Chaining
```c++
std::cout << "Hello, I am " << 22 << " years old!\n";
```
### `std::cin`
- Reads input from keyboard
- `>>`
	- extraction operator 
	- puts the input data in a variable
```cpp
int main()
{
    std::cout << "Input a number: ";

    int x{};
    std::cin >> x;

    std::cout << "Your input number: " << x;
    return 0;
}
```

```cpp
#include <iostream>

int main()
{
    std::cout << "Input your name: ";

    std::string str;
    std::cin >> str;

    std::cout << "Hello " << str << "!";
    return 0;
}
```

```cpp
#include <iostream>

int main()
{
    std::cout << "Input two numbers separated by a space: ";

    int x{};
    int y{};
    std::cin >> x >> y;

    std::cout << "Your entered " << x << " and " << y;
    return 0;
}
```
- Values entered should be separated by whitespace (spaces, tabs, or newlines)
- You have to press enter

### Buffers
- `std::cout` and `std::cin` are buffered
- Outputting data is 2 stage process
	- The data from each output request is added (to the end) of an output buffer (inside `std::cout`).
	- Later, data from (the front of) the output buffer is flushed to the output device (the console).
	- FIFO
- Inputting data is 2 stage process
	- The individual characters you enter as input are added to the end of an input buffer (inside `std::cin`). The enter key (pressed to submit the data) is also stored as a '\n' character.
	- The extraction operator ‘>>’ removes characters from the front of the input buffer and converts them into a value that is assigned (via copy-assignment) to the associated variable. This variable can then be used in subsequent statements.
- Writing data to a buffer is typically fast, whereas transferring a batch of data to an output device is comparatively slow. 
- Buffering can significantly increase performance by batching multiple output requests together to minimize the number of times output has to be sent to the output device.

- Some comment of a redditor
	- This means that data is not immediately sent to the target; instead it is stored in a buffer and sent only when explicitly requested or when the buffer is full
		- `std::flush`
	- Let's say you have 10 lines to print. Let's say sending one line takes 1 ms for the copy and flushing the buffer also takes 1 ms. If you flush after each line, printing everything will take 20 ms ( each line will take 2 ms), if you flush once after everything is written to the buffer, it will take 11ms.
		- So, flushing frequently will make individual lines appear much faster (2ms vs 11ms) but printing everything will be much slower (20ms vs 11ms).

Example
```cpp
#include <iostream>  // for std::cout and std::cin

int main()
{
    std::cout << "Enter two numbers: ";

    int x{};
    std::cin >> x;

    int y{};
    std::cin >> y;

    std::cout << "You entered " << x << " and " << y << '\n';

    return 0;
}
```
- Run 1
	- You enter  `4`, then `4\n` goes to input buffer, and `4`  is extracted to variable `x`
	- You enter  `5`, then `5\n` goes to input buffer, and `5`  is extracted to variable `y`
	-  The program prints `You entered 4 and 5`.
- Run 2
	- You enter `4 5`, then `4 5\n` goes into the input buffer, but  only `4` is extracted to variable `x` (extraction stops at the space).
	- When `std::cin >> y;` is encountered, the program will extract whatever is left in the input buffer, so `5` is extracted to variable `y`. 
	- The program prints `You entered 4 and 5`.
- `>>`
	- operator >> extracts as many consecutive characters as it can, until it encounters either a newline character (representing the end of the line of input) or a character that is not valid for the variable being extracted to.
