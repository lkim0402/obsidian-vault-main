- diagram
	![[Pasted image 20250319123913.png]]
# SQL DB / Relational DB
>[!Definition]
>**RDB (Relational Database) - "Like Excel"** 📊
>- Data is organized into tables with rows and columns, just like a spreadsheet. One row represents one complete data record. They are reliable and great for structured data.
- [[SQL]]
- u should plan the structure ahead of time -> good for maintaining good code quality
	- you add new lines (adding new record)
- *Sharding* 
	- an advanced technique for splitting a massive table across multiple servers to handle huge amounts of data
	- complex to implement correctly
- they can form relationships
	![[Pasted image 20250311204456.png|500]]
#### Popular databases
- oracle db (expensive), MySQL, SQLite, etc (some are open source)
- [[PostgreSQL]] (Postgres)
	- one specific _type_ of database that understands and uses the SQL language
	- a popular and powerful open-source relational database management system
	- you'd use SQL commands (written with the help of the `postgres.js` library) to interact with a PostgreSQL database

# NoSQL DB / Non-relational
>[!Definition]
>**NoSQL - "Like a Notepad"** 📝
>-Newer category of databases that do **not** use a rigid table structure. 
>They are flexible and can store various types of data
- diagram
	![[Pasted image 20250319155824.png]]
- one main advantage: flexibility & scalability
- structure
	- uses things such as key/value pairs, or document models where u store everything in a single document (like a JSON)
- scalability
	- you're able to change the structure of your data afterwards without having to change the entire database
	- not obliged to hold the original structure of the table that was created at the time when you decided to build a users table
	- horizontally (having more fields) & vertically (more records) scalable
- used for real time data (flexible)
- created to address the pain ppl felt using sql db
- popular
	- [[🍃MongoDB]], Redis (open source), [[DynamoDB]] (amazon)