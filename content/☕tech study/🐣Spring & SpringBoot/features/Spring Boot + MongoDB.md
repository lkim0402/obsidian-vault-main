- [[🐣Spring & SpringBoot]], [[🍃MongoDB]]

- Add
	- Spring Data MongoDB module - MongoDB Driver
	- Lombok
	- spring data web
- Add mongodb uri to `application.yml`
	- `spring.data.mongodb.uri=`
- RESOURCES
	- https://www.mongodb.com/resources/products/compatibilities/spring-boot

# `@Document`
- We add the `@Document` annotation -> table name
	- `@Document` annotation is used on a Java class that represents a single **document**, but its purpose is to map that class to a MongoDB **collection** (the name is a bit backwards)
	- the annotation describes the _type_ of thing you are storing
	- "This Java class `Student` is a blueprint for a single document. I want you to store all objects of this type in a collection called `students`"
	- If the collection doesn't exist (this case `students`, MongoDB will create it)
- An instance of your `Student` class = one document
```java
@Document("students")
@Data  
@NoArgsConstructor  
@AllArgsConstructor  
public class Student {  
  
  @Id  
  private Integer rno;  
  private String name;  
  private String address;  
}
```
- `@Id`
	- specifies primary key in our MongoDB document
	- If we don't do this the MongoDB will automatically generate an `_id` when creating the document

# `MongoTemplate` class
- Part of the Spring Data MongoDB module
- a template class that acts as an abstraction layer over MongoDB operations and allows you to perform basic CRUD operations, as well as more complex queries and aggregations

# `MongoRepository` interface
- Part of the Spring Data MongoDB module
- extends the `CrudRepository` & `PagingAndSortingRepository` interfaces
	- [[Spring Data JPA]]
- provides a higher-level abstraction over MongoDB operations than `MongoTemplate`, focusing on the repository pattern to work with data
```java
public interface ItemRepository extends MongoRepository<GroceryItem, String> {
	
	@Query("{name:'?0'}")
	GroceryItem findItemByName(String name);
	
	@Query(value="{category:'?0'}", fields="{'name' : 1, 'quantity' : 1}")
	List<GroceryItem> findAll(String category);
	
	public long count();
	
	// basic CRUD methods provided
}
```

-  `@Query("{name:'?0'}")`
	- requires a parameter for the query, i.e., the field by which to filter the query
- `@Query(value="{category:'?0'}", fields="{'name' : 1, 'quantity' : 1}")`
	- uses the category field to get a particular category's items