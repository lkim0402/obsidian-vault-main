>[!Overview]
>**`ddl-auto`** is a setting that tells your framework (like [[🐣Spring & SpringBoot|Spring Boot]]) how to manages the **schema** (the tables and columns) 
>- Imagine a **robot carpenter** lol 
>- When your application starts, the robot looks at your blueprints (your code) and then looks at the house (your PostgreSQL database).

# Options
- **`create`** 
	- Creates schema on startup (destroys existing data!)
	- drops all existing tables and creates brand new ones from scratch based on your code
	- Great for tests, terrible for production
- **`create-drop`** 
	- Creates schema on startup, drops on shutdown (testing only)
	- Same as `create`, but drops all the tables when the application shuts down. Only used for testing
- **`update`** 
	- Checks the database against your code -> If a table is missing, it creates it. If a column is missing, it adds it. It will **not** delete columns, as that could cause data loss.
	- most common one for development
- **`validate`** 
	- Only validates that schema matches entities (safe)
	- looks at the database tables and compares them to your code. If they don't match perfectly, the application will fail to start
	- Used to make sure that your database schema is correct
- **`none`** 
	- Does absolutely nothing with schema
	- the robot carpenter is turned off lol

# In production
- `validate`
	- best option
	1. **Fail fast** - Find problems at startup, not in production traffic
	2. **Schema drift protection** - Ensures your code matches your database
	3. **Zero risk** - Never modifies your database, only checks it
- `none`
	- unless u have specific reasons to use this use `validate`
	- why u might want to use this:
		- **Database is managed separately** (DBA team handles all schema changes)
		- **Using database migrations** (like Flyway/Liquibase) and want zero JPA interference
		- **Legacy system**  where JPA entities don't perfectly match database