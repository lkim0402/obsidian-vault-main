- more low-level approach than [[C++ Vectors]]
- inherited from C, very rigid
	- you can't add/remove elements, only modify existing elements
- When creating an array, you have to know:
	- the type of data you want to store inside
	- size

```c++
std::array<int, 3> a; // C++ Standard Library
int a[3]; //traditional C-style
```