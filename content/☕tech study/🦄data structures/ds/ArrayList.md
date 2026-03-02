- [[ArrayList in Collections]]
```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    @java.io.Serial
    private static final long serialVersionUID = 8683452581122892189L;

    /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;
```

#### Dynamic resizing
Java's `ArrayList` internally maintains a fixed-size array. If this array no longer has space for more data, it increases its size through the following process (performed automatically).
1. Creation of new Array
	- A new array is created, typically increasing the size of the existing array by about 1.5 times. For example, if the current array size is 10, the new array size will be 15.
	- So, `size()` returns the number of allocated elements, not the total capacity of the array.
2. Copying existing data
	-  All data from the old array is copied to the new array.
	- This copy is a **deep copy**, internally performed using Java's `System.arraycopy()` method.
3. Changing Reference
	- The reference address pointing to the old array is changed to the reference address of the new array.
	- Then, the new array becomes the internal storage space of the `ArrayList`.

#### Performance issues
- Performance Cost of Array Copying
	- Since it's a deep copy, the more data stored the higher the cost
- Potential for irregular performance **spike**
	- generally operations like `add` is very fast but at certain moments (when array is full), internal copying occurs, resulting in a sudden slowdown
- Temporary increase in memory usage
	- when the array's size is expanded (when deep copy occurs) both the old and new array exist simultaneously
	- when large amount of data is stored, the momentary increase in memory usage is greater
- To reduce the cost of `ArrayList`'s dynamic resizing mechanism, you can set an initial capacity when creating an instance
```java
ArrayList<String> list = new ArrayList<>(1000);
```