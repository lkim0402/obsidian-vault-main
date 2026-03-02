# Comparable
>[!Definition]
>**_Comparable_ is an [[Abstraction - Classes & Interfaces#Interface|interface]] defining a strategy of comparing an object with other objects of the same type. This is called the class’s “natural ordering.”**

- Can be passed to a sort method (such as [`Collections.sort`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-java.util.Comparator-) or [`Arrays.sort`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-T:A-java.util.Comparator-)) to allow precise control over the sort order.
	- [[Java Collections Framework (JCF)]]
- Can also be used to control the order of certain [[🦄Data Structures|data structures]] (such as [`sorted sets`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html "interface in java.util") or [`sorted maps`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html "interface in java.util")), or to provide an ordering for collections of objects that *don't have a natural ordering*
- The sorting order is decided by the return value of the _`compareTo()`_ method.

## Example
```java
public class Player {
    private int ranking;
    private String name;
    private int age;
    
    // constructor, getters, setters  

	@Override
	public int compareTo(Player otherPlayer) {
		return Integer.compare(getRanking(), otherPlayer.getRanking());
	}
}
```
- `compareTo()`
	- The sorting order is decided by the return value of `compareTo()`
		- It returns a number indicating whether the object being compared is less than, equal to, or greater than the object being passed as an argument.
	- The _[Integer.compare(x, y)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Integer.html#compare\(int,int\))_ returns -1 if _x_ is less than _y_, 0 if they’re equal, and 1 otherwise.
``` java
public static void main(String[] args) {
    List<Player> footballTeam = new ArrayList<>();
    Player player1 = new Player(59, "John", 20);
    Player player2 = new Player(67, "Roger", 22);
    Player player3 = new Player(45, "Steven", 24);
    footballTeam.add(player1);
    footballTeam.add(player2);
    footballTeam.add(player3);

    // Before Sorting : [John, Roger, Steven]
    System.out.println("Before Sorting : " + footballTeam);
    
    Collections.sort(footballTeam);
    
    // After Sorting : [Steven, John, Roger]
    System.out.println("After Sorting : " + footballTeam);
}
```

# Comparator
>[!Definition]
>A Java [[Abstraction - Classes & Interfaces#Interface|interface]] used to define a custom sorting order for objects

- giving a set of instructions to a sorting method (like `Collections.sort()` or `Arrays.sort()`) on how to compare two objects *when you need more flexibility than the object's "natural" order*
- Example - `Student` object
	- if a class of `Student` objects has a "natural" order to sort by student ID, a `Comparator` lets you create separate, reusable rules to sort those same students by last name, GPA, or age, without changing the `Student` class itself
#### `compare` method
- `compare(T o1, T o2)` => the primary method you have to implement
	- compares two objects (`o1` and `o2`) and returns an integer with the following meaning:
		- **Negative integer**: `o1` should come _before_ `o2`.
		- **Zero**: `o1` and `o2` are equal in terms of sorting.
		- Positive integer: o1 should come after o2

```java
class Student {
    private String name;
    private double gpa;

    // Constructor, getters, and toString()...

    public Student(String name, double gpa) {
        this.name = name;
        this.gpa = gpa;
    }

    // getter, setter
}
```

#### Lambda
```java
import java.util.ArrayList;
import java.util.List;

// ... inside a method
List<Student> students = new ArrayList<>();
students.add(new Student("Eve", 3.9));
students.add(new Student("Charlie", 4.0));
students.add(new Student("Alice", 3.5));

// Sort by GPA (highest first) using a lambda
students.sort((s1, s2) -> Double.compare(s2.getGpa(), s1.getGpa()));
// students list is now sorted: [Charlie (4.0), Eve (3.9), Alice (3.5)]
```

 you can initialize it like this
 -     `Comparator<MyObject> ageComparator = (o1, o2) -> Integer.compare(o1.getAge(), o2.getAge());`

## Other examples
Sorting by string length
```java
String[] words = {"banana", "apple", "grape", "orange"};

Arrays.sort(words, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
});

// [apple, grape, banana, orange]
System.out.println(Arrays.toString(words));  
```

Sorting numbers by ascending order
```java
Integer[] numbers = {5, 3, 8, 1, 2};

Arrays.sort(numbers, new Comparator<Integer>() {
    @Override
    public int compare(Integer a, Integer b) {
        return b - a;  // 내림차순으로 정렬
    }
});

System.out.println(Arrays.toString(numbers));  // [8, 5, 3, 2, 1]
```

By a certain character at a specific index
```java
String[] words = {"sun", "bed", "car"};

Arrays.sort(words, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        int index = 1;  // n번째 인덱스
        char c1 = s1.charAt(index);
        char c2 = s2.charAt(index);
        return Character.compare(c1, c2);
    }
});

System.out.println(Arrays.toString(words));  // [car, bed, sun]ㅈ
```

```java
import java.util.*;

class Solution {
  public String[] solution(String[] strings, int n) {
      Arrays.sort(strings, new Comparator<String>(){
          @Override
          public int compare(String s1, String s2){
              if(s1.charAt(n) > s2.charAt(n)) return 1;
              else if(s1.charAt(n) == s2.charAt(n)) return s1.compareTo(s2);
              else if(s1.charAt(n) < s2.charAt(n)) return -1;
              else return 0;
          }
      });
      return strings;
  }
}
```