- divide your classes into different folders
	- Util folder - contains classes related to utility
	- Each folder is called a package
	- You can have a folder in a folder (package in a package)
- src > main > java
	- package starts from here
- src > main > resource
	- many config/template files listed here

# How do we create packages?

```java
package com.leejun.demo.app*; // importing everything
package com.leejun.demo.app.C; // importing C

public class A {
	public static void main(String[] args){
		C obj = new C();
	}
}
```
- Visual Studio
	- If you click under package > Move to package, then the editor will automatically make the necessary structures and put your file there
	- inside the system explorer, we have the com folder >  leejun folder > .... > 
- You  don't need to import the package of the same class

자바의 대표적인 내장 패키지
- `java.lang`
	- imported automatically
	- contains basic classes like > String, Math, Object 
- `java.util`
	- [[Java Collections Framework (JCF)]], Date class
- `java.io`
	- input, output
- `java.nio`
	- I/O

