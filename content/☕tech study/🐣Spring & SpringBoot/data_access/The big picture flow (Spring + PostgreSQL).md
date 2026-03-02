1. **The Blueprint (Your Code)**
	- You have a `User` class in your project (e.g., a Java class with `@Entity` annotations). This class defines the fields: `id`, `username`, `email`, etc. This is the ultimate source of truth.
2. **The Robot Carpenter (Framework on Startup):**
    - You start your application.
    - Your framework (Spring/Hibernate) wakes up and reads your `application.properties` or `application.yml` file. It sees `spring.jpa.hibernate.ddl-auto = update`.
    - It then scans all your code for blueprints (classes marked with `@Entity`, like your `User` class).
    - It connects to your PostgreSQL database and compares the blueprints to the existing tables.
	    - [[ddl-auto options in yaml]]
	- If it's the 1st time running (no database)
		- The tables are missing, so it generates the `CREATE TABLE users (...)` and `CREATE TABLE products (...)` SQL commands from scratch and runs them
	- If it's NOT the 1st time running (db alr exists)
	    - Since it's set to `update`, it generates and runs the necessary SQL commands (`CREATE TABLE ...`, `ALTER TABLE ...`) to make the database schema match your code.
3. **The API Call (Postman):**
    - You send a `POST` request to `/users` with Postman to create a new user.
    - Your application's **controller code** receives this request.        
    - Your **service/business logic** takes the JSON data from Postman, creates a new `User` object, and tells the framework, "Hey, please save this object to the database."
    - The framework automatically translates that `User` object into an `INSERT INTO users (...) VALUES (...)` SQL statement and sends it to PostgreSQL.
4. **The Database (PostgreSQL):**
    - PostgreSQL receives the `INSERT` command and adds a new row to the `users` table. The data is now permanently stored.

### So, what about your `schema.sql` file?
You have two main ways to manage your schema:
1. **Automatic (The Robot):** Use `ddl-auto: update` and let the framework manage everything based on your entity classes. This is great for fast development.
2. **Manual (The Hand-drawn Blueprint):** Create a `schema.sql` file with the exact `CREATE TABLE` statements you want. You run this file yourself to set up the database. In this case, you would set `ddl-auto: validate` or `ddl-auto: none` to prevent the robot from interfering with your manual work.