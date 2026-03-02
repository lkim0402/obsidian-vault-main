- Reference types are slower than primitive types
	- use primitive types (lowercase)
		- `int, long, float, double`
- Refactor methods
- Early return
- You can use streams instead of for loops, the efficiency is the same and you can reduce time spent on typing code
	- but if you don't remember the syntax just use for loops
# Syntax
## Arrays
```java
import java.util.Arrays;

public class Solution {
	int[] array = {1,2,3,4};
	int[] array2 = new int[] {1,2,3,4};
	int[] array3 = new int[4];
	array3[0] = 1;
	array3[1] = 2;
	array3[2] = 3;
	array3[3] = 4;
	System.out.println(Arrays.toString(array)); // [1,2,3,4]
}
```

## ArrayList (List)
```java
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
list.add(4);

int num = list.get(0); // getting by index
System.out.println(list); // [1,2,3,4]
```

## HashMap
```java
HashMap<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("orange", 3);
System.out.println(map); // {"apple" : 1, "banana" : 2, "orange" : 3}

String key = "apple";
if (map.containsKey(key)) {
	int value = map.get(key);
	System.out.println(key + ": " + value);
} else {
	System.out.println(key + " does not exist");
}

map.remove("orange");
```
- getOrDefault
- computeIfAbsent

## String
- Strings are immutable
	- If you concatenate it, then Java just makes another string and let `String string` point to that new object
```java
String string = "Hello";
string += " World!";
System.out.println(string); // "Hello World!"

string = string.replace("!", "?");
System.out.println(string); // "Hello World?"
```

- `StringBuilder` and `StringBuffer` - use to mutate strings 
	- `StringBuffer` - thread safe (synchronized methods), slower
	- `StringBuilder` - not thread safe, faster << Just use this for leetcode
```java
// make StringBuilder object
StringBuilder sb = new StringBuilder();

// Add
sb.append(10);
sb.append("ABC");

// print
System.out.println(sb); //10ABC
sb.deleteCharAt(3); // 10AC
sb.insert(1,2); // (adding 2 to index 1)
System.out.println(sb); //120AC
```


- compare