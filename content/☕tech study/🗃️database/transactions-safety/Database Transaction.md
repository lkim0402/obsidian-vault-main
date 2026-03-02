- A collection of SQL queries that are treated as *one unit of work*
- When you need to do one or more queries to achieve what you logically want in your application
- Logically one thing that cannot be split
- Can change data / be read-only
- Example: Account deposit (3 queries)
	- `SELECT` : Check if there's enough money from the 1st account
	- `UPDATE`: Deduct `$100` from thyat account
	- `UPDATE` Deposit the money in the other account (+ `$100`)
	- Diagram
		![[transaction.png|700]]
# Transaction Lifespan
- Databases implement all these things differently << Think about how dbs might have implemented things (original thoughts!)
- Transaction `BEGIN`
	- A transaction always starts with `BEGIN`
- Transaction `COMMIT`
	- Actually commits all the changes I made -> makes it persist in the database
	- Think about the work the db is actually doing to persist these changes
	- As a student, how would you build this??
	- *Always think what is happening behind the scene!*
	- What if during commits, it crashes??
- Transaction `ROLLBACK`
	- Just flush anything in the memory =>Undo/Redo space
	- Transaction unexpected ending = `ROLLBACK` (crash)
# Nature of Transactions
- Usually transactions are used to change and modify data
- But you can also have a read only transaction -> *to maintain consistency*
	- You want to generate a report and you want to get consistent snapshot based at the time of transaction
	- If anything changes during the read-only transaction, it's isolated so it's ok