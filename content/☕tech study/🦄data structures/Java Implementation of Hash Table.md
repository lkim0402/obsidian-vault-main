- [[HashMap VS HashSet (Java)]]
# HashSet
> [!Hashset]
> **HashSet** is a structure that stores data using a hash table. 
> - It has the characteristics of a [[Set]], meaning duplicate values are not stored. 
> - Data is stored and accessed using **key values (unique, non-duplicate values)**, not indices.

#### Code
```java
// ex: 중복 제거
HashSet<String> names = new HashSet<>();

names.add("Alice");
names.add("Bob");
names.add("Alice"); // 중복 무시됨

System.out.println(names); // [Alice, Bob]
```

```java
// ex: 빠른 검색
HashSet<Integer> set = new HashSet<>();

for (int i = 0; i < 1000; i++) {
    set.add(i);
}

System.out.println(set.contains(500)); // true
```
# HashMap

>[!HashMap] 
>**HashMap** is the most frequently used class among those implementing the Map interface. 
>- It stores data as **Key-Value pairs**, allowing for various values but requiring keys to be unique (no duplicates). 
>- While data storage can be slow, its use of **hashing** to store data in a hash table makes it excellent for search operations.
#### Code
```java
// ex: 사용자 정보 저장
HashMap<String, Integer> ages = new HashMap<>();

ages.put("홍길동", 25);
ages.put("김철수", 30);

System.out.println(ages.get("김철수")); // 30
```

```java
// ex: 문자 개수 세기
String word = "banana";
HashMap<Character, Integer> counter = new HashMap<>();

for (char c : word.toCharArray()) {
    counter.put(c, counter.getOrDefault(c, 0) + 1);
}

System.out.println(counter); // {a=3, b=1, n=2}
```

# Initial Capacity
```java
// 초기 용량을 지정하여 성능 향상(예상 데이터 수만큼 설정)
HashMap<String, String> map = new HashMap<>(1000);
```
- You can improve performance by specifying an **initial capacity** for your `HashMap` or `HashSet` (set it to roughly the expected number of data items).
- While `HashSet` and `HashMap` internally store data using an array, and that array's size automatically increases as needed, this automatic expansion requires **rehashing** all existing elements. Rehashing can significantly degrade performance.
- You can calculate an appropriate initial capacity using this formula: **Initial Capacity = Expected Number of Data Items / Load Factor**
- Since the **default load factor is 0.75**, if you expect, for example, 1,000 items, an initial capacity of approximately 1334 or more would be suitable.

# Load Factor
```
Load Factor = (Number of Stored Elements) / (Number of Buckets/Array Size)
```
- Acts as a **trigger** for when the hash table needs to **automatically resize (rehash)** itself
	- When the load factor exceeds a certain threshold (0.75 or 75% is the default), the hash table's underlying array is expanded, and all existing elements are re-distributed into the new, larger array
	- This rehashing process is computationally expensive and can cause performance degradation
	- Like a **"fullness alarm"** (rehashing is triggered when array is past this capacity)
#### Why is the default 0.75
- It's chosen to strike a balance between two important performance aspects: **space efficiency** and **search performance (collision avoidance)**

|Load Factor|Characteristics|Advantages|Disadvantages|
|:--|:--|:--|:--|
|**Low** (e.g., 0.5)|Array fills up quickly.|Fewer collisions → Faster lookups|Significant memory waste|
|**High** (e.g., 0.9)|Many elements stored densely.|Good space efficiency|More collisions → Performance degrades|
|**Medium** (0.75)|A compromise between the two extremes.|Suppresses collisions while saving space|Reasonably fast and memory-efficient|
#### Explicitly setting the load factor
You can explicitly set the initial capacity and load factor when creating a `HashMap` or `HashSet`:

```java
HashMap<String, Integer> map = new HashMap<>(128, 0.5f);
```

In this example, the map starts with an initial capacity of 128 buckets, and it will rehash once the number of elements exceeds 128∗0.5=64.

# Correct Implementation of `equals()` and `hashCode()`
- When using **custom (user-defined) objects** as keys in a `HashSet` or `HashMap`, you **must override both the `equals()` and `hashCode()` methods together.**
- If there is no consistency between these two methods, storage and retrieval operations will not function correctly.