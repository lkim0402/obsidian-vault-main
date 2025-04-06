
>[!DynamoDB]
>A fully managed, highly scalable NoSQL key-value database that can maintain millisecond latencies at any scale

- Just like [[RDS (Relational database service)#Aurora|RDS Aurora]], it's a database engine built by Amazon
- Important
	- You don't create tables with the query language that you use to talk to the db
	- No need to connect to the db to then create tables inside of it
	- You NEVER create a db, you JUST create tables!
	- Can create many tables as you need, but you don't group/structure the, into dbs
	- No need to worry about db configurations, AWS takes care for you
- A [[AWS Infrastructure (Regions, AZs, Edge locations)#Region|regional service]] 
- ex) A user wants a NoSQL database that can scale to millions of requests per second with single-digit millisecond latency
## You create tables
> You don't create instances or clusters unlike [[RDS (Relational database service)|RDS]] or [[ElastiCache]], just start right at the table lvl
- You have to give the table a name
- Partition key
	- Main identification key
	- specify a primary key that uniquely identifies each item in the table
	- should be unique for each item in the table, like an ID
- Choose payment
	- Non on demand plan
		- Choose expected read/write capacities to define how much you're gonna pay for
	- On demand 
		- you only pay for the occurring read/writes, more expensive but more flexibility
- Choose encryption settings on a table basis 
- Once a table is created/configured
- start managing data
- write/read data with the [[Different Ways of Accessing AWS|AWS API/CLI/SDK]]
	- no general query language, but u need to use AWS tools to interact
- configure backups for the data
## Advanced Features
- diagram
	![[Pasted image 20250319163839.png]]
- Streams
	- DynamoDB Streams is essentially a log of changes (additions, updates, and deletions) made to items in a DynamoDB table
	- not enabled by default
	- allows you to capture changes to items in your DynamoDB table and make these changes available for other systems to process or take action upon
	- useful when you need to **react to data changes** (like additions, updates, or deletions) in real-time without polling the database constantly
		- kinda reminds me of [[== React ==|React]]!
	- After enabling Streams, other systems or applications can subscribe to the stream and be notified whenever there is a change.
- Global tables
	- A Global Table is a special type of DynamoDB table that spans multiple [[AWS Infrastructure (Regions, AZs, Edge locations)|AWS regions]]. It automatically replicates the data between these regions.
	- You can convert a table from one region into a global table
	- makes it available with low-latency access for users anywhere in the world
	- Not free (and can get quite expensive)
	- Click table > create replica
- DAX
	- Database Acceleration feature
	- A managed in-memory cache layer, fully managed by AWS
	- does come at a price