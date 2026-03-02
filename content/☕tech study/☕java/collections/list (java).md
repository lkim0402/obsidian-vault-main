| Method              | Modifiable? | Fixed size? | Java Version |
| ------------------- | ----------- | ----------- | ------------ |
| `Arrays.asList()`   | Partially   | ✅ Yes       | Java 1.2     |
| `new ArrayList<>()` | ✅ Yes       | ❌ No        | Always works |
| `List.of()`         | ❌ No        | ✅ Yes       | Java 9+      |

```java
List<String> list1 = Arrays.asList("a", "b", "c");
List<String> list3 = List.of("a", "b", "c");

List<String> list2 = new ArrayList<>(Arrays.asList("a", "b", "c"));

```