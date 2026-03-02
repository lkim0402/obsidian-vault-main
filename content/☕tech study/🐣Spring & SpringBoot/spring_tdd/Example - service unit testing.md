- Rewriting the note for this because my other notes about unit testing are a MESS rn

# Quickstart (basics)
```java
@ExtendWith(MockitoExtension.class)
public class ProductServiceTest {
	@Mock
	ProductRepository productRepository;

	@InjectMocks
	ProductService productService;
	
	@Test
	void addProductShouldAddProductSuccessfully() {
		// given
		Product product = new Product();
		product.setId(1);
		product.setName("Book");
		Mockito.when(productRepository.save()).thenReturn(product);
		
		// when
		Product addedProduct = productService.addProduct(product);
		
		// then
		Assertions.assertNotNull(addedProduct);
		Assertions.assertEquals(product.getId(), addedProduct.getId());
		Assertions.assertEquals(product.getName(), addedProduct.getName());
		Assertions.assertTrue(product.getId()==1);
	}
	
	@Test
	public void deleteProductShouldDeleteSuccessfully() {
		// given
		doNothing().when(productRepository).deleteById(1);
	
		// when
		productService.deleteProduct(1);
		
		// then
		Mockito.verify(productRepository, times(1)).deleteById(1);
	}	
}
```
- `addProductShouldAddProductSuccessfully`
	- `InjectMocks`
		- injects the necessary mocks, for example the `productService` needs a `ProductRepository`
	- Because the `productRepository` is a mock, when we save something using the service it will return `null`
		- So we basically have to mock the save method that's called in the service
		- We do that using `Mockito`'s `.when`
			- if method returns: `when` + `.thenReturn`
			- if method doesn't return: `doNothing().when`
	- Now we make the assertions
		- `assertNotNull()`
		- `assertEquals()`
		- `assertTrue`
		- You can just import `org.junit.jupiter.api.Assertions` too lol
- `deleteProductShouldDeleteSuccessfully`
	- `doNothing().when(productRepository).deleteById(1);`
	- `Mockito.verify`
		- different verification methods: `times(n)`, `never()`, `atLeastOnce()` etc (you can see in intellij)

# Testing private methods
```java
	// testing private methods
@Test
void testPrivateMethod_validateProductName() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
	Method validateProductName = ProductService.class.getDeclaredMethod("validateProductName", String.class);
	validateProductName.setAccessible(true); // provided by java reflection
	
	Boolean book = (Boolean) validateProductName.invoke(productService, "Book");
	
	assertTrue(book); // for invalidation, you can use assertFalse(book)
}
```
- `void testPrivateMethod_validateProductName()` - *testing private methods*
	- Use  *Java Reflections*!
	- Just adds the exceptions of whatever it wants lol
# Testing exceptional scenarios
```java
@Test
void addProductShouldThrowExceptionForInvalidProductName() {
	// given
	Product product = new Product();
	product.setId(1);
	product.setName("");
	
	// when
	Product addedProduct = productService.addProduct(product);
	
	// then
	assertThrows(RuntimeException.class, () -> {
		productService.addProduct(product);
	})
	assertEquals("Invalid Name Of Product", runtimeException.getMessage());
	verify(productRepository, times(0)).save(any(Product.class));
}
```