
>[!OSI model]
>Open Systems Interconnection Reference Model
>- Describes the broad overview (not the details) of how data traverses our networks
>- NOT the OSI protocol suite
>	- but we can apply to many different protocols & it works perfectly with the TCP/IP protocols we use today
>- There are unique protocols at every layer of the OSI layer

- When we're talking with other ppl we can refer to the issues to specific layers so they know exactly what we're talking about
- As data is passed down the OSI model, it's encapsulated into more and more components
- Anything below the point that you are communicating with is abstracted away

# Networking Stack

>All People Seem To Need Data Processing
>Please Do Not Throw Sausage Pizza Away
- diagram
	![[osi_layer.png]]
	- Host layer
		- how the data is chopped & reassembled for transport
		- how the data is formatted for both sides of the network connection
	- Media layer
		- how data is moved between point A and B (in the same local network or across the internet)
- The software that does each of these functions(layers) -> networking stack
	- your phone, laptop, wifi, the server has a network stack
- A layer X device means that it has the capabilities of layer X AND below
### Layer 7: Application
- The layer we see on our screen
- common operations that operate here
	- HTTP, FTP, DNS, POP3
### Layer 6: Presentation
- Putting all data we can read & understand
	- character encoding
	- application encryption
- often combined with the application layer
- [[Layer 5 - Session]]
- [[Layer 4 - Transport]]
- [[Layer 3 - Network]]
- [[Layer 2 - Data Link]]
- [[Layer 1 - Physical]]

# Diagram

OSI model to real world

| Layers          |                                                                         |
| --------------- | ----------------------------------------------------------------------- |
| 7: Application  | Your eyes                                                               |
| 6: Presentation | Application Encryption (SSL/TLS)                                        |
| 5: Session      | Control protocols, tunneling protocols                                  |
| 4: Transport    | TCP segment, UDP datagram                                               |
| 3: Network      | IP address, router, packet                                              |
| 2: Data Link    | MAC address, Frame, Switch, Extended Unique Identifier (EUI-48, EUI-64) |
| 1: Physical     | Cables, fiber, and the signal itself                                    |

# Wireshark
- lets u capture the data in your network
- 3 screens
	- 1st - frame by frame breakdown
	- 2nd - more detail in the single frame
	- 3rd - hexadecimal & ASCII breakdown of the data