
> [!Multi-AZ]
> - An AWS feature that improves database **availability** and **fault tolerance** by automatically replicating data across multiple [[AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|AZs]]
> - If the primary database instance fails, AWS **automatically fails over** to a standby replica, minimizing downtime

- You need to ensure there is no single point of failure -> prevent the chance of failure
- fail-over
	- when you have to shift traffic to a redundant system in case the primary system fails
- common example is having a copy of your [[Databases (AWS)|db]] where all ongoing changes are synced
	- the copy is not in use until a failover occurs and it becomes the primary db