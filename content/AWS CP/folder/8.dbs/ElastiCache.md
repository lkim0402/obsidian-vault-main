
>[!ElastiCache]
>A fully managed in-memory caching database

- diagram - caching
	![[Pasted image 20250319153751.png|500]]
- diagram - ElastiCache
	![[Pasted image 20250319154121.png|500]]
- A caching database
	- A db that helps you store that caching data in memory
	- In memory caching
		- All about storing data that's frequently requested in memory, so that it can be retrieved quicker than a typical database
- Not a traditional db
	- Technically, ElastiCache can store data and serve as a data store, but it is not a database in the traditional sense
	- While it can store and retrieve data, it lacks several key features that typical databases (like [[RDS (Relational database service)|RDS]] or [[DynamoDB]]) provide, which makes it unsuitable as a full-fledged database for persistent data storage
- You need to choose [[Virtual Private Cloud (VPC)|VPC, subnets]], and [[Security groups|security groups]]
- ElastiCache query commands

# Main Engines
- Redis
	- open-source, in-memory key-value data store
	- often used as a cache but can also function as a general-purpose data store
- Memcached
	- also an open-source, in-memory key-value store
	- Unlike Redis, Memcached is simpler and focuses solely on caching
	- designed to be a high-performance, distributed memory cache that speeds up web applications by alleviating database load
- Valkey
	- open-source, in-memory, high-performance key-value datastore that serves as a drop-in replacement for Redis OSS
