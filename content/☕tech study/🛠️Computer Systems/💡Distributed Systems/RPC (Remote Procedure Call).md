
>[!Overview]
>A request from a client to execute a function on a server
>- When a computer program causes a procedure to execute *in a different address space* (perhaps on another computer) -> *location independent*
>- To the client, looks like a procedure call
>- To the server, looks like an implementation of a procedure call

- Diagram
	 ![](https://www.c-sharpcorner.com/article/getting-started-with-remote-procedure-call/Images/Server2.jpg)
# (Review) Local procedure call
- The caller and the callee are both in the same program (in same)
- Caller invokes function (by name) with arguments
	- Pass some arguments in registers, push others
	- Push return program counter
	- jump to first instruction in callee
- Callee (implementation of function)
	- Reads parameters from registers and stack
	- Computes function, possibly updates state, returns result
	- Jumps back to next instruction in caller
- Example
	- Caller: `if (BuyBook(OSPP)) print(“Done!”);` 
	- Callee: `Boolean BuyBook(char \*booktitle) { do stuff; return success; }`
	- With an RPC, the `BuyBook` function would be running on a completely different computer
- Compiler defines protocol: name binding, where to find arguments, etc.
# Remote procedure call (RPC)
- Client request to execute a function on the server
	- The goal is to *hide the network communication from the developer*. 
		- The developer just writes `result = BuyBook(OSPP)`, and the RPC framework handles all the complex network steps in the background.
- On client: `result = BuyBook(OSPP)`
	- Parameters **marshalled** into a message (arbitrary types)
		- **Marshalling** (also called "**serializing**") = the process of taking the parameters (in this case, `OSPP`) and converting them into a standardized byte format that can be sent in a network message
	- Message sent to server (may be multiple pkts)
		- The byte stream (the "message") is broken down into network packets and sent over the network (TCP/IP) to the server's address
	- Wait for reply
- On server: implement `BuyBook` - The server has the _actual_ `BuyBook` function code, it is listening for incoming network requests
	- message is **parsed**
		- **Parsing** (also called "unmarshalling" or "deserializing") = the process of receiving the stream of bytes from the network and converting it back into the original data structures the function understands (e.g., the string `"OSPP"`).
	- Perform operation (executes local `BuyBook("OSPP")`) & gets result
	- Put result into a message (may be multiple pkts)
	- Result returned to client
- The client (which was waiting) receives the message, parses (unmarshalls) the result, and its original function call `result = BuyBook(OSPP)` completes
## Why Serialize?
- Procedure arguments can be values or pointers
	- Need to be assembled into a linear message
- What should happen if you call an RPC with a pointer?
	- Pass the pointer? Convert it to a global reference? 
	- The RPC framework (the "**stub**" code that is auto-generated) detects that the argument is a pointer and follows one of two strategies, depending on how the pointer is being used.
	- File write - Pass a copy of the data it points to
		- Most common solution
		- Dereferences the pointer (follows it to the actual data), serializes (copies) that data, and puts the copy of the data into the network message
	- File read -Store the result
## Implementation
![[RPC_implementation.png]]

# RPC vs Procedure Call
> Problems RPCs have that LPC (local procedural calls) don't have
- Binding
	- Client needs a connection to server
	- Server must implement the required function
	- What if the server is running a different version? (ex, function accepts different parameters)
- Service Discovery Service
	- Solution to binding problem
	- Like a phonebook - Includes what services are available, including versions and packet formats (may be 1000s of APIs) 
	- When server starts it registers itself here, when client makes a call it asks this service
- Interface Description Language (IDL) 
	- An **IDL** (like Google's **Protobuf**) is a file that acts as a language-neutral **contract**. You define the functions and, most importantly, the exact structure of the data.
	- automatically writes all the complex serialization/deserialization code for you, programmer just uses the generated classes
- Failures
	- What happens if messages get dropped? Duplicated? Delayed? Reordered?
	- What if client crashes? 
	- What if server crashes? 
	- What if server crashes after performing op but before replying?
	- What if server appears to crash but is slow?
	- What if network partitions?
	- (461 alert) Some are handled by TCP but what if TCP socket fails?
	- The Naive RPC is a simple implementation that is *vulnerable to almost all failure listed above*
# Naive RPC
- The most basic implementation you could ever build (problematic)
- [[Lab Framework#Node (Abstract class)|Nodes]]
	- Any number of clients and servers
	- *No state* at client or server
	- Server performs operation if it gets a message
- [[Lab Framework#Message|Messages]]
	- Client requests, server replies
	- Client request contains IP address of client/server, name of procedure, arguments
	- Server reply contains IP address of server/client, results
- Timers: none
## What can go wrong?
- Message corruption
	- Delivered msg might be different: Becomes “buy Silberschatz” instead of “buy OSPP”
	- 📌Fix: include checksum or CRC or cryptographic signature in each message
- Message dropped
	- Request message dropped (due to failure)
		- operation not performed, client waits forever 
	- Reply message dropped
		- operation was performed, but client doesn’t know if operation was performed, waits forever
	- 📌Fix with client timer and retransmit
- Request message duplicated
	- operation performed twice
- If multiple requests, and some are dropped
	- client doesn’t know which operation was done and which ones aren’t
	- 📌Fix: use unique ID per request: (client ID, sequence number)
- Message ordering
	- Network congestion
		- Client sends sequence of msgs to server (`a,b,c,d..`) (using threads) but some can get dropped (network congestion)
		- If it's a sequence for CRUD (delete, create, then fill new file), if the sequence is mixed then it can't do it properly
	- Network reorders packets, server gets different sequence
	- 📌Fix: Label messages with sequence number
		- `{seq: 1, op: "delete"}`
		- Detect missing messages & unneeded retransmissions
	- Labs assume each client sends only one RPC at a time
		- If client sends the next RPC, it means they must have received the reply for the previous one 
		- Still need to worry about lost and duplicate and delayed RPCs
		- E.g., server might get a delayed request
# RPC Semantics
- Semantics = meaning
- How many times will an RPC be executed by the server before it returns to the invoker of the RPC?
- At least once (NFS, DNS) 
	- Client retries
	- If reply: executed at least once
	- If no reply: maybe executed, maybe multiple times
- At most once
	- • If reply: executed once
	- If no reply: maybe executed, but never more than once
	- Server prevents non-unique requests from compound 
	- UID (sequence numbers like 1,2,3,4) 
		- [Don’t use UUID.randomUUID] 
	- [[Lab Framework#Idempotency and Determinism|Idempotent]]
- Exactly once
	- If reply: executed once 
	- If no reply: keep waiting
	- At most once with retries
# Research Papers
- [[A Cloud-Scale Characterization of Remote Procedure Calls (FOCI)]]