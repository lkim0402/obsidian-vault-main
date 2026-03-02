
>[!layer4]
>Ensures **reliable or efficient delivery** of data **between applications** on different devices
>- ensures the data reaches the **correct application** (e.g., browser, game, chat app) on that device using **port numbers**
>- [[OSI 7-layer model]]
- diagram
	![[Pasted image 20250428112153.png]]
- Transport information from one device to another
	- Make sure the data arrives correctly, completely, and in order
	- doesn't care where the destination is (that's layer 3's job), but it cares how the data moves between the 2 devices reliably
- The "post office" layer (like parcels/letters)
	- getting your information from one side of the network to another
- can divide a large file or msg into segments so they can be sent over the network
	- not about "fitting into the network size" but more about managing and organizing communication cleanly
		- **Layer 4:** "Here’s a big essay. I’ll chop it into organized pages to send."
		- **Layer 3:** "Oh, the pages are too big for the envelope! I’ll slice them into smaller pieces to fit."
	- also responsible for putting them back together
# Sessions & state
![[sessions-state.png]]
- stateless firewall
	- [[Network Access Control Lists (NACLs)]]
	- Each packet is evaluated in isolation
	- **Two rules per connection**:
	    - One rule for the **initial request** (e.g., from your laptop to a server).
	    - One rule for the **response** (e.g., from the server back to your laptop).
	- The firewall doesn't "understand" the connection's context (i.e., it doesn’t know if the response should match an outbound request). It simply checks each packet on its own.
- stateful firewall
	- The firewall knows if the initial request has been made and can automatically allow the corresponding response (since it's part of the same connection).
	- **Example**: If you allow the initial outbound connection to a server, the firewall will automatically allow the response **back in** without needing a separate rule.
#### Traffic
- outbound
	- data that is **leaving** your device or network and going to **another device or server** -> You need a rule allowing that outbound connection.
	- Ex) Your laptop sending a request to a server
- inbound 
	- data coming **back** to your device or network from another device or server -> You need a rule allowing the inbound response
	- Ex) After your laptop sends a request to a server (outbound), the server sends the response (like the website data) back to your laptop
# Protocols
- Protocols (2 ways of sending data reliably or quickly)
- both run on top of IP and add a collection of features
	- [[Layer 3 - Network#Internet Protocol (IP)|Layer 3 - IP]]
## TCP (Transmission Control Protocol)
>[!TCP]
>It is a connection based protocol.
>- A connection is established between two devices using a **random** port on a client and a **known** port on the server.
>	- [[Backend & Main Components Explained#Client-side vs Server-side|Backend Explained -Client-side vs Server-side]]]]
>- Once established, the connection is **bi-directional**
>- The connection is a reliable connection, provided via the segments encapsulated in IP packets
- Reliability, order correction, ordering of data
- Used for most of the important lab protocols (HTTP, HTTPS, SSH, etc)
	- [[HTTP requests in Express]]
- Connection oriented protocol
	- connection between 2 devices, once set up it creates a bidirectional channel of communications
### Architecture
![[tcp-architecture.png]]
- **TCP Stream/Channel**: This represents a continuous, ordered flow of data between two devices. It's established using TCP segments, which are the individual units of data transmission
- **Ports**
	- Ephemeral port /high ports- a temporary port number assigned by the client's operating system for the duration of a session
		- typically **in the range 49152–65535**
	- Well-known port - a standardized port number used by servers to listen for incoming connections
		- range 0–1023
- from layer 4 perspective, **two sets of segments** are created because the data flows in two directions 
	- one with source with ephemeral & dest with well known
	- vise versa
- often you will need to add firewall rules to allow the ephemeral/high port range back to the client
- so that's why u need 2 sets of rules on a network ACL within AWS
	- (Access Control List) ACL - operates at OS/resource lvl 

### Segments
![[tcp-segment.png]]
- TCP HEADER
	- encapsulated within [[Layer 3 - Network#IP Packets|IP packets]]
		- the segments are INSIDE the packets
		- the packets carry the segments from their source to their destination
	- Source port and destination port
		- has source port (sender’s app) and destination port (receiver’s app)
		- You can have **multiple conversations** between the same two devices (same IPs), but for **different apps** or tabs — each with a different port
		- Each conversation is a unique combination of the source and destination IP and the source and destination port
	- Sequence number
		- HOW MUCH DATA HAS BEEN SENT FOR THE SESSION
		- it is incremented with each segment that's sent (ant it's unique)
		- IP packets can be reordered correctly when received!
	- Acknowledgements
		- indicates that is has received cumulated data and is ready for the next segment
		- every segment which is transmitted needs to be acknowledged
			- if device 1 is sending segment 1,2,3,4 to device 2, then device 2 needs to acknowledge that it's received them 
		![[tcp-seq-ack-flow-e1524681839913.webp]]
		- the length here is the payload size
		- `SEQ = 1` ->the tcp segment length is 669: 669 bytes of data were sent in that TCP segment
		- each blue block is a frame (+ IP packet + TCP segment)
	- Flags 'N' Things (9 bits)
		- (flags + number of extra fields)
		- various controls over the TCP segments and the wider connection
		- used to close the connection or to synchronize the sequence numbers
		- flags that can be set to influence your connection
			- 
	- window
		- the number of bytes you indicate that you're willing to receive between acknowledgements
		- once reached, the sender will pause until you acknowledge that amount of data 
		- large windows are more efficient because the header of a TCP segment
	- checksum
		- error checking
	- urgent pointer
- AND THE DATA
### TCP Connection 3-way Handshake
>**SYN, SYN-ACK, and ACK** are part of the **initial connection setup**. They are used at the beginning of a TCP connection, during the **3-way handshake**.

![[tcp-3way.png]]
- Once the 3-way handshake is complete, the **connection is established**, and the devices (client and server) can begin transferring **actual data**.
- **Initial Sequence Number (ISN)**
	- When two devices (sender and receiver) first establish a TCP connection (this is called a **3-way handshake**), both agree on an **Initial Sequence Number** (ISN). This is usually randomly selected.
		- Since both devices are sending and receiving data, they each use their **own ISN** to track the data they send
- **Subsequent Sequence Numbers**
	- The sequence number of each segment is then calculated by the number of bytes sent.
	- If the first segment starts with **sequence number 1000**, and it contains **100 bytes**, the sequence number of the next segment will be **1100** (1000 + 100).
	- If the next segment contains **150 bytes**, its sequence number will be **1250** (1100 + 150), and so on.
- After the connection is established through the **3-way handshake**, both the client and the server **"pick up" from the sequence numbers** they created during the handshake, and they continue to use those sequence numbers for data transfer

## UDP (User Datagram Protocol)
- Faster, all about performance
- less reliable
- No TCP overhead required for the reliable delivery of data