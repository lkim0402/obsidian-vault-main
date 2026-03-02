- Related: [[Java Serialization (and persistence)]]
# Working with Files 

In Java, the `File` class from the `java.io` package is used to **abstract and handle files and directories**.
```java
// new File("data.txt"): Creates a `File` object representing the file `"data.txt"`.
File file = new File("./data.txt");

// getAbsolutePath(): Returns the full path to the file.
System.out.println("File path: " + file.getAbsolutePath());
// exists(): Checks whether the file or directory actually exists in the filesystem.
System.out.println("Does the file exist? " + file.exists());
```
# Java I/O Overview
- The **main API** for I/O operations:
	- `java.io` → traditional, blocking I/O
	- `java.nio` → newer, non-blocking I/O (NIO = New I/O)
## I/O Stream
- Java handles **input and output (I/O)** through **streams**, which allow you to read/write data sequentially (like a **pipe**).
- Different from [[Stream in Java]]
## Kinds of I/O Streams
![[fileio-dragram.png|500]]

| Type                | Description                                 | Examples                      |
| ------------------- | ------------------------------------------- | ----------------------------- |
| **Input**           | Reads data *into the program*               | `InputStream`, `Reader`       |
| **Output**          | Writes data *out of the program*            | `OutputStream`, `Writer`      |
| **Byte-based**      | For *binary* data (e.g., images, raw files) | `InputStream`, `OutputStream` |
| **Character-based** | For *text* data (UTF-16 characters)         | `Reader`, `Writer`            |
## Classification: Node vs Filter Streams

### Node Streams (기본 스트림)

> *Directly connected* to a data source or destination (file, memory, network)

| Type       | Input Stream  | Output Stream  |
| ---------- | ------------- | -------------- |
| Byte-based | `InputStream` | `OutputStream` |
| Char-based | `Reader`      | `Writer`       |
- When reading data by **byte**: `InputStream`
	![[inputstream.png]]
- When writing data by **byte**: `OutputStream`
    ![[outputstream.png]]
- When reading data by **character**: `Reader`
    ![[reader.png]]
- When writing data by **character**: `Writer`
	![[writer.png]]
###  Filter Streams (보조 스트림)

> Wrap around node streams to **add features** like buffering, formatting, etc.

| Purpose                               | Input Stream        | Output Stream        |
| ------------------------------------- | ------------------- | -------------------- |
| Buffering                             | `BufferedReader`    | `BufferedWriter`     |
| Character conversion                  | `InputStreamReader` | `OutputStreamWriter` |
| Reading/writing primitives            | `DataInputStream`   | `DataOutputStream`   |
| Object serialization (object streams) | `ObjectInputStream` | `ObjectOutputStream` |
- Most `BufferedXXX` classes in Java I/O wrap around a non-buffered "node stream" (like `FileWriter`, `InputStream`, etc.) — they don’t work alone.
```java
BufferedWriter writer = new BufferedWriter(new FileWriter("scores.txt"));
writer.write("hello");
writer.close();
```
- `FileWriter` opens a **real connection to the file**.
- `BufferedWriter` wraps it to **buffer character output**, reducing slow disk writes.

Example: `ObjectOutputStream`
```java
FileOutputStream fos = new FileOutputStream("channelName.ser");
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeObject(channel); // writes the object in binary format
```
- object streams serialize Java objects into a special binary format that Java can deserialize later
- It's not the same as writing text or raw bytes—you’re preserving object structure, including fields and types
	- more on serialization later
### Examples (Byte-based)
Using `FileInputStream` - reading data from the files
```java
import java.io.FileInputStream;

public class FileInputStreamExample {
    public static void main(String[] args) {
        try {
            FileInputStream fileInput = new FileInputStream("java.txt");
            int i;
            while ((i = fileInput.read()) != -1) {
                System.out.print((char) i);
            }
            fileInput.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

Using `BufferedInputStream`
```java
import java.io.FileInputStream;
import java.io.BufferedInputStream;

public class FileInputStreamBuffered {
    public static void main(String[] args) {
        try {
            FileInputStream fileInput = new FileInputStream("java.txt");
            BufferedInputStream bufferedInput = new BufferedInputStream(fileInput);
            int i;
            while ((i = bufferedInput.read()) != -1) {
                System.out.print((char) i);
            }
            bufferedInput.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```
- `BufferedInputStream` uses buffer internally

Using `FileOutputStream` - create a file (if it doesn't exist) and write data into it
```java
import java.io.FileOutputStream;

public class FileOutputStreamExample {
    public static void main(String[] args) {
        try {
            FileOutputStream fileOutput = new FileOutputStream("java.txt");
            String word = "ja";
            byte[] b = word.getBytes();
            fileOutput.write(b);
            fileOutput.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}

```
### Examples (Char-based)
#### Saving List data into txt
- use `flush()` and `close()`
	- `flush()` - puts data in buffer to destination
```java
List<String> names = List.of("Alice", "Bob", "Charlie");

BufferedWriter writer = null;

try {
    writer = new BufferedWriter(new FileWriter("names.txt"));
    
    for (String name : names) {
        writer.write(name);
        writer.newLine();
    }
    
    writer.flush();
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (writer != null) {
        try {
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
- You can use a set too (`Set.of` instead of `List.of`), but the order is not guaranteed
#### Saving Map data into txt file
```java
Map<String, Integer> scoreMap = Map.of("Tom", 90, "Jane", 85, "Paul", 95);

BufferedWriter writer = null;

try {
    writer = new BufferedWriter(new FileWriter("scores.txt"));
    
    for (Map.Entry<String, Integer> entry : scoreMap.entrySet()) {
        writer.write(entry.getKey() + ":" + entry.getValue());
        writer.newLine();
    }
    
    writer.flush();
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (writer != null) {
        try {
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Saving data from txt file into List
```java
List<String> names = new ArrayList<>();

BufferedReader reader = null;

try {
    reader = new BufferedReader(new FileReader("names.txt"));
    String line;
    
    while ((line = reader.readLine()) != null) {
        names.add(line);
    }
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (reader != null) {
        try {
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Saving data from txt file into Map
```java
Map<String, Integer> scoreMap = new HashMap<>();

BufferedReader reader = null;

try {
    reader = new BufferedReader(new FileReader("scores.txt"));
    String line;
    
    while ((line = reader.readLine()) != null) {
	    // this is the part that changed!
        String[] parts = line.split(":");
        if (parts.length == 2) {
            scoreMap.put(parts[0], Integer.parseInt(parts[1]));
        }
    }
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (reader != null) {
        try {
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

