
>[!treeset]
>**TreeSet** is a type of **[[Set]]** collection in [[☕Java]] that stores elements **in sorted order**

- Just like [[TreeMap (Java)]], it uses a **[[Red Black Tree (balanced BST)]]** internally

	- This structure keeps operations efficient (`O(log n)`) and ensures elements remain sorted automatically.
# Characteristics
- **Does not allow duplicates**, just like any other Set.
- **Maintains elements in sorted order**, but it does **not preserve insertion order**.
- It implements the `NavigableSet` interface, enabling:
    - Efficient **range queries** (e.g., find all elements between two values),
    - Easy access to **smallest/largest** elements,
    - Other navigation-related methods.

# Example
>Example: Storing integer data in an automatically sorted state
- when you add integers to a TreeSet, they are stored in such a way that the set keeps the numbers sorted automatically — **you don’t have to manually sort the elements**
```java
public static void main(String[] args) {
    // 정수형 데이터를 자동 정렬 상태로 저장
    TreeSet<Integer> set = new TreeSet<>();
    
    set.add(40);
    set.add(10);
    set.add(30);
    set.add(20);

    System.out.println(set); // 출력: [10, 20, 30, 40]

    // 범위 검색 기능 활용
    System.out.println("30 이상: " + set.tailSet(30)); // [30, 40]
    System.out.println("첫 번째 요소: " + set.first()); // 10
}
```

> Example: Storing string data sorted in alphabetical order
- automatically keeps everything sorted based on the alphabetical (lexicographical) order of the strings
```java
public static void main(String[] args) {
    
    // 문자열 데이터를 알파벳 순으로 정렬하여 저장
    TreeSet<String> names = new TreeSet<>();
    
    names.add("Charlie");
    names.add("Alice");
    names.add("Bob");
    
    System.out.println(names); // 출력: [Alice, Bob, Charlie]
    
    // 사전순 검색 기능
    System.out.println("Bob보다 큰 이름들: " + names.tailSet("Bob")); // [Bob, Charlie]
    
    // 반복자(iterator)를 이용해 하나씩 대문자로 변경해서 출력처리
    Iterator<String> iter = names.iterator();
    
    while(iter.hasNext()){
        System.out.println(((String) iter.next()).toUpperCase());
    }
    
    // 배열로 바꾸어 연속 처리하기
    Object[] arr = names.toArray();
    
    for(Object obj : arr){
        System.out.println(((String) obj).toUpperCase());
    }
}
```

>Example: Lottery generator
```java
public static void main(String[] args) {
    
    Set<Integer> lotto = new TreeSet<>();

    // 1-45 숫자 랜덤으로 6번 추출
    while(lotto.size() < 6) {
		    // lotto는 TreeSet 타입이므로 중복값을 허용하지 않기 때문에
		    // 중복된 숫자가 add() 되어도 무시되어 size()는 그대로 유지됨
        lotto.add(((int) (Math.random() * 45)) + 1);
    }

    System.out.println("lotto : " + lotto);
}
```
- `Math.random()`
	- gives number between 0 to 1