
>[!Overview]
>MapStruct is a library that helps in converting [[Data Transfer Object (DTO)|DTOs]] and Entities.
>-  Automatically generates the implementation for a mapper interface at compile time, giving you fast, type-safe, and boilerplate-free code for your conversions.
>- If you use this well, *your working time will decrease tremendously!*
# Initialization
- [[Gradle#DI (Dependency Injection) Dependency in Gradle Dependency Configuration|Gradle dependency configuration]]
```groovy
dependencies {
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'

	// add these
    implementation 'org.mapstruct:mapstruct:1.4.2.Final'
    annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.2.Final'
}
```

# Example
>Product -  The Entity
```java
@Data
@Entity
@Getter
public class Product {

    @Id
    private Long id;
    private String name; // <<<
    private String description;
    private double price;
}
```

>ProductDto - The Dto
```java
@Data
public class ProductDto {
    private Long id;
    private String productName; // << Different name from the entity
    private String description;
    private double price;
}
```

> The Mapper
```java
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.factory.Mappers;

@Mapper(componentModel = "spring")
public interface ProductMapper {

    // You can define an instance for non-Spring usage if needed
    // ProductMapper INSTANCE = Mappers.getMapper(ProductMapper.class);

    /**
     * Maps a Product Entity to a Product DTO.
     * The field 'name' in Product is mapped to 'productName' in ProductDto.
     * Fields 'id', 'description', and 'price' are mapped automatically by name.
     */
    @Mapping(source = "name", target = "productName")
    ProductDto toDto(Product product);

    /**
     * Maps a Product DTO back to a Product Entity.
     * The field 'productName' in the DTO is mapped back to 'name' in the Entity.
     */
    @Mapping(source = "productName", target = "name")
    Product toEntity(ProductDto productDto);
}
```

- `@Mapper(componentModel = "spring")`
	- tells MapStruct to generate an implementation that is a Spring [[Bean]]
	- You have to put this on top of every mapper
- `source`, `target`
	- source = from
	- target = to

# NOT a proxy 
- Mapstruct is an annotation processor that runs during your project's **compile time** (ex. when you build your project using [[Gradle]] or maven)
- What happens
	- It scans your code with [[Abstraction - Classes & Interfaces#Interface|interfaces]] with `@Mapper`, reads the method signatures and `@Mapping` annotations inside that interface
	- It then generates a concrete Java implementation class for that interface! => Ex. `ProductMapperImpl.java`
	- This generated class is plain [[☕Java|java]] that performs the getter & setter calls, and is also compiled into bytecode along with the rest of the code
		- [[JVM, JRE, JDK & the Compilation Process]]
- [[AOP (Aspect Oriented Programming)]]


- MapStruct vs ModelMapper
	- ModelMapper - 리플렉션을 써서 동적으로 가지고 와서 매핑해줌. 
	- MapStruct - 훨씬 빠름. 클래스파일 만듬 -> 빌드 -> build folder에 classes/member/mapper/mapstruct -> **구현 클래스가 만들어져있음!!!**
	- **빌드되는 시점에 구현 클래스를 만들어줌**
	- 클래스: 기본생성자 + setter / 전체생성자 / 빌더 << 다 대응을함
		- 클래스에 다 있으면, mapper은 뭘 쓸지 헷갈려함
			- 빌더 > 전체생성자 > 기본생성자 
			- 아무것도 없으면 안만들어짐
- proxy아님!

default 쓰는 것 대신 바로 알려주는 것
- annotation 사용

