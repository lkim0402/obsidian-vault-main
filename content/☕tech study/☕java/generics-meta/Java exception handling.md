```
            Throwable
               |
        ------------------
        |                |
      Error           Exception
                       |
           -------------------------
           |                       |
   Checked Exception       Unchecked Exception
  (compile-time)            (runtime exception)

```
# Error VS Exception
#### Errors
- we **CANNOT** handle it (didn't happen because of us)
- usually something we can't even fix thru code
- examples
	- some problem with your hardware
	- out of memory error
	- stack overflow
#### Exception
- we **CAN** handle it
- Types
	- Runtime exception / Unchecked Exceptions
		- Not checked at compile time , happens  during runtime.
		- Often due to programming errors like logic issues
		- Ex) `NullPointerException`
	- non-runtime exception / Checked exception
		- enforced at compile -> must be caught with `try-catch` or declared with `throws` 
		- The compiler **forces** you to handle them.

#### Syntax Errors (Separate)
- NOT `Throwable` objects (not part of the `Error` vs `Exception` hierarchy)
- They happen before the program runs — at compile time.
	- You must fix syntax errors or your program won’t compile
# Exception handling
- We don't want the code to stop running due to runtime errors => handle the exception
- Exceptions are actually a class

```java
public static void main(String args[])
{
	int i = 4; //normal
	int j = 0; //normal
	int result;
	int nums = {2,3,4};
	
	try
	{
		result = i/j;
		System.out.println(nums[4]);
	} 
	// catch(Exception e)
	catch(ArithmeticException e)
	{
		System.out.println("ArithmeticException!");
	}
	catch(ArrayIndexOutOfBoundsException e)
	{
		System.out.println("ArrayIndexOutOfBoundsException!");
	}
	catch(Exception e)
	{
		System.out.println("Something went wrong.");
	}
	finally
	{
		System.out.println("BYE"); 
	}
	
}
```
- `finally` is NOT like else  - it's GUARANTEED to happen
	- to close files
		- every time u open a file it's your responsibility to close it
	- release resources
	- clean up memory
	- log completion

if you want to throw the error by yourself
```java
if (result==0)
{
	throw new ArithmeticException();
}
```
- since Exception is a class, you need to create an object of the exception

# throws
Used in a **method declaration** to indicate that the method **might throw** exceptions.

```java
public void divide(int a, int b) throws ArithmeticException {
    if (b == 0) {
        throw new ArithmeticException("Division by zero");
    }
    System.out.println(a / b);
}

```