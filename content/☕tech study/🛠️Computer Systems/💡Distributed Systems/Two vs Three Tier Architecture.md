# Two-tier Architecture
 - Scalable number of front-end web servers
	 - Stateless (“RESTful”): if crash can reconnect the user to another server
	 - Q: how is the user mapped to a front-end?
 - Scalable number of back-end database servers
	 - Run carefully designed distributed systems code
	 - If crash, system remains available
	 - Q: how do servers coordinate updates?
# Three-tier Architecture
- Scalable number of front-end web servers
	- Stateless (“RESTful”): if crash can reconnect the user to another server 
- Scalable number of cache servers
	- Lower latency (better for front end)
	- Reduce load (better for database)
	- Q: how do we keep the cache layer consistent? 
- Scalable number of back-end database servers 
	- Run carefully designed distributed systems code