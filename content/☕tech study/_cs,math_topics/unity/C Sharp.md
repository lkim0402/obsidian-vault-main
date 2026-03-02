### Types
```C#
// types
string PlayerName = "Kate"
int age = 10;
float age = 10.2;
bool isAlive = true;

//declaring methods
void CleanYourRoom()
{
	//Thigs to add;
}
//void is the return type
//there are no arguments

//calling your method
CleanYourRoom();
```
- Conventions
	- functions start caps, variables start lowercase
	- All methods go under `Start()` and `Update()`

### Classes
- They are "containers" for variables and methods that allow us to group similar things together
- One object has 1 script, 1 class
	- New class each time we create a new script and have that script responsible for one thing
- Unity has many classes already created for us
	- https://docs.unity3d.com/2020.1/Documentation/ScriptReference/Input.html
- `public` VS `private
	- `public`
		- It's accessible from other scripts. This means that other classes can create instances of this class, call its methods, or access its fields and properties
	- `private`
		- It's only accessible within the same file or within the same class if it is an inner class. It cannot be accessed from other scripts, and you cannot attach it as a component in the Unity Editor
		- default to `private` unless stated `public`
- `Input.GetKey` VS `Input.GetKeyDown`
	- `Input.GetKey`
		- used to check if a specific key is being held down during the current frame. It returns `true` for every frame that the key is pressed.
		- Useful for continuous input
	- `Input.GetKeyDown`
		- used to detect when a key is pressed down **once** in a given frame. It returns `true` only during the first frame when the key is pressed.
		- ideal for actions that should only happen once per key press, such as jumping or firing a weapon
- `Ctrl + Shift + Space`
	- In VS Code it gives  back the info of the method/class
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehavior
```
-  Inheritance: We're deriving from `MonoBehavior`

### Namespace in C# 
- A way to organize and group related classes, interfaces, enums, and other types.
- Helps prevent naming conflicts by differentiating between classes with the same name that might be found in different libraries or parts of your program. They also make your code easier to read and manage.
- `using UnityEngine;`
	- `UnityEngine` is the primary namespace provided by Unity that contains most of the core classes and functions you need to interact with the Unity engine.

### Switch syntax
```C#
switch (variableToCompare)
{
	case valueA:
		//some action
		break;
	case valueB:
		//some action
		break;
	default:
		//other action
		break;
}
```

### Conventions
In Unity, C# code is arranged:
- Variables
	- Parameters - for tuning, typically set in the editor (similar to [[Parameter|parameters]] in [[Machine Learning|ml]])
	- cache - e.g. references for readability or speed ([[Caching]])
	- state - private isntance (member) variables
- Start
- Update
- public functions
- private functions