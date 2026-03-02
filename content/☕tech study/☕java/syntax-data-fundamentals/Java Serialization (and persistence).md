- Related: [[Java File IO]]
# Serialization
>Converting objects to storable format
- A specific type of file I/O operation
- After Serialization → Stored as Bytes
	- When you serialize an object, Java converts it into a stream of bytes that represents:
		- The object's class information
		- All its field values
		- Metadata needed for reconstruction

| Regular File I/O                                          | Serialization                                                          |
| --------------------------------------------------------- | ---------------------------------------------------------------------- |
| Saves data as **text** or **raw bytes**<br>               | Saves **entire Java objects** directly to files                        |
| You manually convert your **data to strings/bytes**       | Java **automatically** converts **objects to a special binary format** |
| Example: Saving a Map as "key:value" lines in a text file | Much more convenient for complex objects!                              |
![[fileio-dragram.png|500]]
## Serialization Uses the Same I/O Streams

```java
FileOutputStream fos = new FileOutputStream("channelName.ser");
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeObject(channel); // serialize the object
```
- `FileOutputStream` - the node stream (connects to file)
- `ObjectOutputStream` - the filter stream (adds serialization capability)

## With VS Without serialization
#### Without (manual approach)
```java
// Saving a Person object manually
Person person = new Person("Alice", 25, Arrays.asList("Java", "Python"));

BufferedWriter writer = new BufferedWriter(new FileWriter("person.txt"));
writer.write(person.getName() + ":");
writer.write(person.getAge() + ":");
writer.write(String.join(",", person.getSkills()));
writer.close();
```

#### With Serialization (automatic)
```java
import java.io.Serializable;

public class Person implements Serializable {
		private static final long serialVersionUID = 1L;
		// 이하 생략
}
```
- The class should `implements Serializable` 
- The `serialVersionUID` field represents the version of a class when performing serialization and deserialization
	- If the version of the class you're trying to deserialize is different from the version of the class you expect, deserialization will fail
- The static, long, and the variable name (serialVersionUID) are mandatory rules that must be followed. While private and final aren't strictly required, they are strongly recommended.

Use `ObjectOutputStream` to serialize
```java
Person person = new Person("김철수", 25);

// 파일에 객체 직렬화하기
try (FileOutputStream fos = new FileOutputStream("person.ser");
     ObjectOutputStream oos = new ObjectOutputStream(fos);
) {
    oos.writeObject(person);
} catch (IOException e) {
    e.printStackTrace();
}
```
- `.ser` extension is used

# Deserialization
> Deserialization is the reverse process: **reading the bytes and reconstructing the original object**.

Use `ObjectInputStream` to deserialize
```java
try (FileInputStream fis = new FileInputStream("person.ser");
     ObjectInputStream ois = new ObjectInputStream(fis)) {
    Person person = (Person) in.readObject();
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

User `transient` keyword for excluding fields you don't want to serialize
```java
public class Person implements Serializable {
		private static final long serialVersionUID = 1L;
    private String name;
    private transient String password; // 직렬화에서 제외됨
}
```
# Full example
```java
// Step 1: Serialize (Object → Bytes)
Person original = new Person("Bob", 30);
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"));
oos.writeObject(original);
oos.close();

// Step 2: Deserialize (Bytes → Object)  
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"));
Person restored = (Person) ois.readObject();
ois.close();

// They're equivalent but different object instances!
System.out.println(original.getName().equals(restored.getName())); // true
System.out.println(original == restored); // false (different objects in memory)
```

# Exceptions
> Key Exceptions During Serialization/Deserialization

- `IOException`
	- This exception occurs during general input/output operations.
- `ClassNotFoundException`
	- You'll see this when the system can't find the class during deserialization.
- `InvalidClassException`
	- This one pops up when the `serialVersionUID` doesn't match during deserialization.

# Persistence
>[!Persistence]
>**Making data persistent"**
> - storing data so it survives after your program ends

- Making data survive beyond program execution
- Always involves permanent storage (files, databases)
### Types of Persistence:

1. **File Persistence** (what you're learning):
    - Save to text files, binary files, serialized files
    - Data survives program shutdown
2. **Database Persistence**:
    - Save to databases (MySQL, PostgreSQL, etc.)
    - More robust for large applications

# Closing stream (Wrapping)
```java
  @Override
public List<UserStatus> findAll() {
	try {
		return Files.list(DIRECTORY) // get all the files in DIRECTORY as a list
				.filter(path -> path.toString().endsWith(EXTENSION))
				.map(path -> {
					// mapping each file to ois.readObject()
					try {
						FileInputStream fis = new FileInputStream(path.toFile());
						ObjectInputStream ois = new ObjectInputStream(fis);
						return (UserStatus) ois.readObject();
					} catch (IOException | ClassNotFoundException e) {
						throw new RuntimeException(e);
					}
				}).toList();
	} catch (IOException e) {
		throw new RuntimeException(e);
	}
}
```
- This results in memory leak
- Wrap in `()` instead
```java
@Override
public List<UserStatus> findAll() {
	try {
		return Files.list(DIRECTORY) // get all the files in DIRECTORY as a list
				.filter(path -> path.toString().endsWith(EXTENSION))
				.map(path -> {
					// mapping each file to ois.readObject()
					try (
						FileInputStream fis = new FileInputStream(path.toFile());
						ObjectInputStream ois = new ObjectInputStream(fis)
					) {
						return (UserStatus) ois.readObject();
					} catch (IOException | ClassNotFoundException e) {
						throw new RuntimeException(e);
					}
				}).toList();
	} catch (IOException e) {
		throw new RuntimeException(e);
	}
}
```