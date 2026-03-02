
>[!Overview]
>- Distributed Systems are a group of computers that work together to accomplish some task
>	- Independent failure modes
>	- Connected by a network with its own failure modes

- [[Intro - Reasons and Challenges]]
- [[Course project for Distributed Systems - key-value store]]
- [[Two vs Three Tier Architecture]]
- [[Lab Framework]]
- [[RPC (Remote Procedure Call)]]

- Sources
	- MIT Distributed Systems Playlist
		- Paper for every lecture, the lectures aren't going to make much sense if you haven't read the papers. Read a paper rapidly and efficiently
	- UW 452 Distributed Systems

- Infrastructure - Abstractions
	- Storage
	- Communication systems
		- as a tool computers need to communicate
	- Computation system
- Implementation
	- RPC
		- goal is to mask the fact that we're communicating over an unreliable network
	- Threads
		- Programming technique that allows us to harness multi-core computers
		- Way of structuring concurrent operations
	- Locks
		- Because we're going to spend lots of time on concurrency control
- Fault Tolerance
	- Generally
		- If a system has a single computer, it can stay up for years without crashing, it's pretty reliable
		- However, if a system has 1000 computers, you will have many failures a day
		- Big scale turns events into constant problems
		- The ability to mask failures, the ability to proceed without failures *has* to be built into the design
	- Availability
		- Some systems are designed so that under some failures, the system will keep operating despite the failure
		- A good available system needs to be recoverable as well
	- Recoverability
		- If something goes wrong, maybe servers will stop working
		- The system should be able to recover and continue as if nothing happened (w/o any loss of correctness)
		- They need to save the data 
	- Tools for fault tolerance
		- Non volatile storage
			- hard drives to store a checkpoint or log a state of a system
			- Then its recovered, you can read the saved data
			- Management - its expensive to update... so u need clever ways to write to non-volatile storage so you won't write it a lot
		- Replication
			- Replicated copies that has same system state
			- Problem - the replicas will not be in sync and not be replicas
- Consistency
	- Maybe we're building a KV service, and it has
		- `put(k,v)`
		- `get(k) -> v`
	- If we're replicating the state, we need to think about how to keep consistency among replicas
	- There are strongly consistent and weakly consistent systems
		- strong
			- guaranteed to see the updated data
			- very expensive spec to implement, lots of communication
			- Also we want the failures to be independent....
		- weak 
			- makes no guarantee
			- sometimes this is useful because its less expensive
		- 