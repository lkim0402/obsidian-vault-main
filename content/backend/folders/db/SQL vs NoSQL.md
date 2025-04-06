- diagram
	![[Pasted image 20250319123913.png]]

## SQL DB / Relational DB
- Structured Query language
- sort of like a programming language that allows you to query a database that is in this particular structured format
- the database that has been around for years
- u should plan the structure ahead of time -> good for maintaining good code quality
- data stored in **tables**
	- you add new lines (adding new record)
- they can form relationships
	![[Pasted image 20250311204456.png|500]]
- popular databases
	- oracle db (expensive), postgres, MySQL, SQLite, etc
	- some are open source

## NoSQL DB / Non-relational
- diagram
	![[Pasted image 20250319155824.png]]
- one main advantage: flexibility & scalability
- uses things such as key/value pairs, or document models where u store everything in a single document (like a JSON)
- scalability
	- you're able to change the structure of your data afterwards without having to change the entire database
	- not obliged to hold the original structure of the table that was created at the time when you decided to build a users table
	- horizontally (having more fields) & vertically (more records) scalable
- created to address the pain ppl felt using sql db
- popular
	- [[MongoDB]], Redis (open source), [[DynamoDB]] (amazon)