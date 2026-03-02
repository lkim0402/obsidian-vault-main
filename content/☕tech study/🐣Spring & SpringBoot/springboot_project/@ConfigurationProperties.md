
>[!Role]
>Facilitates the binding of properties defined in external configuration files—most commonly `application.properties` or `application.yml`—to a [[POJO vs JavaBeans VS Spring Beans#POJO (Plain Old Java Object)|POJO]]
- Recommended to isolate configuration properties into separate 
# Basics

`application.yml`
```yaml
#Simple properties
mail.hostname=host@mail.com
mail.port=9000
mail.from=mailer@mail.com

```

`ConfigProperties.java`
```java
@Configuration <<<
@ConfigurationProperties(prefix = "mail")
@Getter
@Setter
public class ConfigProperties {
    
    private String hostName;
    private int port;
    private String from;

    // standard getters and setters
}
```

>[!Note]
>If you remove `@Configuration` in the [[POJO vs JavaBeans VS Spring Beans#POJO (Plain Old Java Object)|POJOs]], you need to put `@EnableConfigurationProperties(ConfigProperties.class)` in the main Spring application class to bind the properties into the POJO.

The alternative way:
```java
@SpringBootApplication
@EnableConfigurationProperties(ConfigProperties.class)
public class EnableConfigurationDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(EnableConfigurationDemoApplication.class, args);
    }
}
```

- Then Spring will automatically bind any property defined in our property file that has the prefix mail and the same name as one of the fields in the *ConfigProperties* class.
- 

>[!Note]
>Since SpringBoot 2.2, Spring finds and registers `@ConfigurationProperties` class via classpath scanning, enabled by  explicitly adding the `@ConfigurationPropertiesScan`
>- we don’t have to annotate such classes with `@Component` (and other meta-annotations like `@Configuration`),or even use the `@EnableConfigurationProperties`

```java
@ConfigurationProperties(prefix = "mail") 
public class ConfigProperties { 

    private String hostName; 
    private int port; 
    private String from; 

    // standard getters and setters 
}
```

Main file (ex. `DiscodeitApplication.java)
```java
@SpringBootApplication  
@ConfigurationPropertiesScan  
public class DiscodeitApplication {  
  
    public static void main(String[] args) {
```
- `@ConfigurationPropertiesScan`
	- This enables  `@SpringBootApplication`'s classpath scanner to find the _ConfigProperties_ class, even though we didn’t annotate that class with `@Component`
	- Can also scan custom locations
		- Ex. `@ConfigurationPropertiesScan("com.baeldung.configurationproperties")`
# Nested Properties
Let’s create a new _Credentials_ class to use for some nested properties:
```java
public class Credentials {
    private String authMethod;
    private String username;
    private String password;

    // standard getters and setters
}
```

We also need to update the _ConfigProperties_ class to use a _List,_ a _Map_, and the _Credentials_ class:
```java
public class ConfigProperties {

    private String hostname;
    private int port;
    private String from;
    private List<String> defaultRecipients;
    private Map<String, String> additionalHeaders;
    private Credentials credentials;
 
    // standard getters and setters
}
```

The following properties file will set all the fields:
```yaml
#Simple properties
mail.hostname=mailer@mail.com
mail.port=9000
mail.from=mailer@mail.com

#List properties
mail.defaultRecipients[0]=admin@mail.com
mail.defaultRecipients[1]=owner@mail.com

#Map Properties
mail.additionalHeaders.redelivery=true
mail.additionalHeaders.secure=true

#Object properties
mail.credentials.username=john
mail.credentials.password=password
mail.credentials.authMethod=SHA1
```

# Using it in @Bean
**We can also use the _@ConfigurationProperties_ annotation on _@Bean_-annotated methods.**
- This approach may be particularly useful when we want to bind properties to a third-party component that’s outside of our control.
- Let’s create a simple _Item_ class that we’ll use in the next example:
```java
public class Item {
    private String name;
    private int size;

    // standard getters and setters
}
```

Using _@ConfigurationProperties_ on a _@Bean_ method to bind externalized properties to the _Item_ instance:
```java
@Configuration
public class ConfigProperties {

    @Bean
    @ConfigurationProperties(prefix = "item")
    public Item item() {
        return new Item();
    }
}
```

Consequently, any item-prefixed property will be mapped to the _Item_ instance managed by the Spring context.
# Property Validation
**_@ConfigurationProperties_ provides validation of properties using the JSR-380 format.**
```java

public class ConfigProperties {

	@NotBlank
    private String hostname;
    @Min(1025)
    @Max(65536)
    private int port;
    
	@Pattern(regexp = "^[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,6}$")
    private String from;
    private List<String> defaultRecipients;
    private Map<String, String> additionalHeaders;
    private Credentials credentials;
 
    // standard getters and setters
}
```

```java
public class Credentials {
	@Length(max = 4, min = 1)
    private String authMethod;
    private String username;
    private String password;

    // standard getters and setters
}
```
- Helps to reduce lots of *if-else* in our code
- If any of these fail, then the main application would fail to start with an _IllegalStateException_
# Sources
- https://www.baeldung.com/configuration-properties-in-spring-boot