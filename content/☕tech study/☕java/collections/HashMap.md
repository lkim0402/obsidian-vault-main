```java
HashMap<String, Pokemon> pokedex = new HashMap<>();
pokedex.put("피카츄", new Pokemon("피카츄")); // put
Pokemon p = pokedex.get("피카츄"); // get

for (String key : pokedex.keySet()) {
    System.out.println(pokedex.get(key));
}
```
- `Map` is an interface
	- The class that implements map is `HashMap`
- The keys are basically a set