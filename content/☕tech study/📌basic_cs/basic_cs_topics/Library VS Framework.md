
>[!Main differences]
>- Library = Like reusable Tetris blocks — you choose when and how to use them
>- **Framework** =  Like a Tetris board with cutouts — you _must_ fit the blocks into the given structure

# Example

| Language       | Library        | Framework                                                |
| -------------- | -------------- | -------------------------------------------------------- |
| [[Python]]     | Pandas, Numpy  | Django, Flask                                            |
| [[JavaScript]] | Axios          | [[React]], [[☕tech study/full stack/next.js]]                                   |
| [[☕Java]]      | Apache Commons | Spring Framework, [[Java Collections Framework (JCF)\|Collections Framework]] |
| C#             | Math.NET       | Unity                                                    |

- When choosing a framework/library to learn, you need to choose carefully what you will learn because you will most likely take lots of time into learning

# Library
- Libraries are usually specific to certain programming languages ([[Python]] - Pandas)
- You bring code that you want (which someone else wrote) and add it to your project
- Use when you need to add specific functionality
# Framework
>**Framework** in programming generally provides a **specific mold or structure for programming**
- Use when you want to build structured application with less boilerplate
- Advantage
	- It includes features that I might not have thought of
	- Reduces development time.
	- Standardized nature allows for an expected level of quality.
	- Easier maintenance.
- Disadvantages
	- over-reliance can diminish a dev's abilities
	- learning curve per framework
- Basically
	- Programmers initially wrote each application from scratch using a specific programming language, but as more applications were made it was easier to observe that many of these apps had similar requirements, such as:
		- Logging error, warning, and info messages
		- Transactions
		- Protection mechanisms against the same common vulnerabilities
		- Mechanisms for performance increase, like caching or data compression
	- It turned out that the *business logic code* implemented in an app is *significantly smaller* than the stuff that makes the engine of the application!
## In an app (containing business logic code)
- *Business logic code*
	- Code that implements the business requirements of the application, it's what implements the user's expectations in an application
	- Ex) "clicking on a specific link will generate an invoice"
	- Business logic code is what makes an app different from another 
- But an app has much more features
	- The user's perspective is similar to viewing an iceberg! => they observe the results of the business logic code, but it's only a small part
	- Various applications can just reuse the other features (caching, data transfer, etc)
- diagram
	![[outside_business_logic.png]]
- 
## Types of Frameworks

| Category                         | Description                                                                                                                            | Examples                             |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------- |
| **Persistence Framework**        | Frameworks that provide classes and configuration files as libraries for handling data storage, retrieval, modification, and deletion. | - `Mybatis`<br>- `Hibernate`         |
| **Java Framework**               | Frameworks that modularize and provide necessary elements, focusing on web application development through Java EE.                    | - Spring Framework<br>- Struts       |
| **UI Implementation Framework**  | Frameworks that provide a structure to facilitate easier front-end implementation.                                                     | - Bootstrap<br>- Foundation<br>- MDL |
| **Function & Support Framework** | Frameworks that offer features to assist with specific functionalities or task execution.                                              | - Log4j<br>- JUnit <br>- ANT         |
