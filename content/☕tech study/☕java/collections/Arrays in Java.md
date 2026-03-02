# Basics
```java
int [] intArray = {0,0,0,0,0} 
int [] intArray = new int[5] // same as above, initializes to 0

int[] intArray;
intArray = new int[5]; // 크기 5의 빈 배열

int[] intArray = {1, 2, 3, 4, 5};
```

## Useful
- `length`
	- getting the length of array
- `Arrays.toString(array)`
	- Changes the array to string
```java
int[] arr = {1,2,3,4,5};
System.out.println(arr.length); // 5
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));
```

## Nested arrays
```java
int[][] arr = {{1,2,3}, {4,5,6}};
System.out.println(arr[1][2])

// change value
arr[1][2] = 7;
System.out.println(arr[1][2]); // 7
```
## Aliasing
```java
int[] arr1 = {1,2,3,4,5};
int[] arr2 = arr1;
```
- `arr1` and `arr2` point to the same address, so `arr2` is an *alias* to `arr1`

## Cloning
```java
int[] arr1 = {1, 2, 3, 4, 5};
int[] arr2 = arr1.clone();

arr1[0] = 100;
System.out.println(arr1[0]); // 100
System.out.println(arr2[0]); // 1
```

## For-each
```java
for (int i : intArray)
{
	System.out.println(i);
}
```
- X getting the index but the element itself

# Different ways of creating arrays
`List.of`
- Makes it immutable
```java
List<String> list = List.of("A", "B", "C");
```

`Arrays.asList(...)`
- Fixed-size List backed by an array
```java
List<String> list = Arrays.asList("A", "B", "C");
```

`new ArrayList<>()` 
- Mutable List
```java
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
```

