- Source
	- https://foci.uw.edu/papers/sosp23-rpc.pdf
# What this paper studies
- This is the **first large-scale study of RPCs in a real production environment** - specifically Google's infrastructure supporting services like Search, Gmail, Maps, and YouTube. The researchers analyzed:
	- Over 700 billion RPC samples
	- 10,000+ different RPC methods
	- Data collected over nearly 2 years (23 months)
## Key Findings
1. Why is RPC Evaluation Important?
	1. RPCs are Growing Rapidly
		- RPC usage is increasing ~30% annually
		- RPC throughput is growing faster than compute resources
		- This puts huge demands on network and compute infrastructure
2. Not all RPCs are the same
	- **Popularity is skewed**:
	    - Top 10 methods account for 58% of all calls
	    - Top 100 account for 91% of calls
	    - The single most popular RPC ("Network Disk Write") is 28% of all calls
	- **But slow RPCs matter too**: The slowest 1,000 methods take 89% of total RPC time despite being only 1.1% of calls
3. Nested RPCs are Wider than Deep
	- RPCs often trigger chains of other RPCs (nested calls)
	- These call trees tend to be **wide** (many parallel calls) rather than **deep** (long chains)
	- Median: 13 descendants per RPC
	- Tail can be huge: 90% have P90 counts over 105 descendants
4. RPC Size Matters
	- Most RPCs are small (median ~1.5 KB)
	- But there's a huge tail: P99 requests are 196 KB, responses are 563 KB
	- Most RPCs are write-dominant (sending more data than receiving)
5. Storage RPCs are Important
	- Network Disk and database systems (Spanner, Bigtable) generate most traffic
	- These storage services handle the most RPCs and transfer the most data
	- But they use proportionally less CPU than compute-intensive services
6. "RPC Latency Tax" Breakdown**
	- The "tax" is everything except application processing time:
	- **On average**: Only 2% of total time
		- Network: 1.1%
		- RPC processing: 0.49%
		- Queuing: 0.43%
	- **BUT at the tail (P95)**: The tax becomes much more significant
		- For 10% of methods, the tax is 38%+ of total time
	- Application processing dominates on average, but network and queuing matter a lot at the tail
7. Different Services Have Different Bottlenecks
	- They studied 8 major services and found:
		- **Application-heavy**: Bigtable, Network Disk, F1, ML Inference, Spanner (processing time dominates)
		- **Queuing-heavy**: SSD cache, Video Metadata (waiting in queues is the problem)
		- **RPC-stack-heavy**: KV-Store (RPC overhead itself is significant)
8. Geographic Distribution Adds Unavoidable Latency
	- Cross-datacenter RPCs are limited by speed of light
	- When client and server are far apart, network latency dominates
	- Main issue is **lack of data locality** (data isn't where it needs to be)
9. CPU Costs
	- RPCs consume ~7.1% of all CPU cycles fleet-wide
	- Biggest consumers: compression (3.1%), networking (1.7%), serialization (1.2%)
	- High variation in CPU cost per RPC (heavy-tailed distribution)
10. Load Balancing Issues
	- Load is significantly imbalanced across clusters
	- Better load balancing could improve performance
	- Challenge: Hard to predict which RPCs will be expensive
11. RPC Errors Are Costly
	- 1.9% of RPCs fail
	- Most common: Cancellations (45%), often from "hedging" (sending duplicate requests to reduce tail latency)
	- Cancelled RPCs waste 55% of error-related CPU cycles
# Major Implications
1. **For researchers**: Assumptions about microsecond-scale RPCs are wrong for real systems
2. **Optimization priorities differ by service**: No one-size-fits-all solution
3. **Queuing matters**: Better scheduling could significantly reduce tail latency
4. **Storage is critical**: Storage RPCs dominate traffic volume
5. **Hardware accelerators**: Could help with compression, encryption, serialization
6. **Application-specific approaches needed**: Different services need different optimizations

This paper provides crucial real-world data that challenges many assumptions and should guide future datacenter and RPC system design.
