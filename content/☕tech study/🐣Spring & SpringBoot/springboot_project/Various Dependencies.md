# Optional Dependency
- In situations where a [[Bean]] may or may not exist, optional injection is necessary
#### `@Autowired(required = false)`
```java
@Autowired(required = false)
private NotificationService notificationService;
```
- *prevents an exception from being thrown* during the injection process even if the specified Bean does not exist
- constructor injection cannot be used here since that implies that the [[Bean]] is *absolutely required* for the object's creation
	- see [[DI (Dependency Injection)#Methods#Constructor Injection|constructor injection]]
#### `Optional<T>`
```java
@Autowired
private Optional<NotificationService> notificationService;
```
#### `@Nullable`
```java
@Autowired
@Nullable
private NotificationService notificationService;
```

# Collection/List Dependencies
- When multiple Beans of the same type exist, Spring can automatically inject them into a `List`, `Map`, or array
	- **`List<T>`**: All Beans corresponding to the type `T` are injected (automatic sorting is by Bean registration order).
	- **`Map<String, T>`**: Beans are injected with the Bean name as the key and the Bean object as the value.
	- **`T[]`**: Beans of the same type are injected as an array.
- But we will probably never use this lol
#### List
```java
@Service("pokemonServiceCollection")
public class PokemonService {
	
	/* 1. List 타입으로 주입 */
	private List<Pokemon> pokemonList;

	@Autowired
	public PokemonService(List<Pokemon> pokemonList) {
		this.pokemonList = pokemonList;
	}

	public void pokemonAttack() {
		pokemonList.forEach(Pokemon::attack);
	}
}
```
#### Map
```java
@Service("pokemonServiceCollection")
public class PokemonService {
	
	/* 2. Map 타입으로 주입 */
	private Map<String, Pokemon> pokemonMap;
	
	@Autowired
	public PokemonService(Map<String, Pokemon> pokemonMap) {
		this.pokemonMap = pokemonMap;
	}
	
	public void pokemonAttack() {
      pokemonMap.forEach((k, v) -> {
          System.out.println("key : " + k);
          System.out.print("공격 : ");
          v.attack();
      });
  }
}
```