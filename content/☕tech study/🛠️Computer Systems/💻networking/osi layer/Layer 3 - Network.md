
>[!layer 3] 
>Moves data between different local networks (inter-networking)
>- **Routes packets** between **different networks** (LANs).
>- Uses **logical addressing**, like **IP addresses**, instead of physical ones (like MAC addresses).
>- Enables data to travel **across the internet** or between distant networks.

limitations
- every packet is single & isolated!
- doesn't know **which program or app** on the destination device the packet is meant for
	- (e.g., browser, Discord, Zoom)
- no flow control
	- if the source transmits faster than the destination can receive, it can saturate the destination causing packet loss
- packets can be delivered out of order
	- no guarantee that the packets will take the same route from source to destination 
	- because of network conditions, they could arrive in a different order
- packets can go missing
	- network outages/conditions which cause temporary routing loops
	- limited number of hops
- network conditions can cause delay in delivery
# P2P Link
>A **Point-to-Point (P2P)** link is a **direct connection** between two devices (or two routers/networks)

- Example
	- Ethernet cable between two routers
- It’s fine for small setups, but not scalable
	- You’d need a new cable or configuration for every pair of connected networks
# Why u need layer 3
- Layer 2 (like Ethernet) is limited to a single LAN
- You can't simply connect 2 LANs together if they use different layer 2 protocols (Ethernet vs Wi-Fi, or ATM vs. PPP)
- Layer 3 solves this by
	- providing a IP address, a universal addressing scheme
	- routing data from 1 LAN to another using routers that operates at layer 3
- ![[{1A0F7D84-D492-4AAD-918B-D6D1F986BBB1}.png]]
# Internet Protocol (IP)
> [!definition]
> A Layer-3 protocol which adds cross-network IP addressing and routing to move data between LANs without direct P2P links
## Concepts
- IP Packets are moved from source to destination across the internet through many intermediate networks
- Routers (layer 3 devices) move packets of data across different networks
- **Layer 3 IP Packet**: Contains the source & destination **IP addresses**
- **Layer 2 Frame**: Wraps the IP packet and includes the source & destination **MAC addresses**
- What happens when the packet travels across networks
	- As it goes **from router to router**, the **Layer 2 frame is stripped and rebuilt at each hop**. Only the **IP packet stays the same**.
	- At each hop, **new MAC addresses** are used (source = router's MAC, destination = next hop’s (next router's) MAC).
	- But the **IP source/destination stays constant**.
## IP Packets
![[IP packet.png]]
- Contains:
	- IP source address (where packet is generated) and IP destination address 
	- Protocol field
		- has [[Layer 4 - Transport|layer 4]] protocol (ex. TCP)
		- the destination will know which layer 4 protocol to pass the data into
	- The data - from layer 4
	- TTL (time to live)
		- the packet will live through many different networks
		- defines how many hops the packet can hop before being discarded
		- used to stop packets looping around forever
		- (in v6 - called Hop Limit)
	- and other fields
- packets remain the same as they move across networks
- versions 4 and 6
	- 4 - used for decades
	- 6 - more scalable
## IP address
> It is what identifies a device which uses layer 3 IP networking

- either statically assigned by humans (static IP) or assigned automatically by machines (DHCP)
	- DHCP - dynamic host configuration protocol
- IP addresses need to be unique
#### Subnet Masks
![[subnet-masks.png]]
- Used to determine which part of an IP address is the **network address** and which part is the **host address**
- Devices use subnet masks to check if a destination is on the **same network** or if it must forward to a **gateway**
- steps to determine network/host portions
	- **Convert the IP and subnet mask** to binary.
	- Perform a **bitwise AND** between the IP and the subnet mask.
	- The result is the **network address**
 - **Network address (starting address)**:  
	- Network bits + all `0`s in host portion
- **Broadcast address (ending address)**:  
	- Network bits + all `1`s in host portion

#### IP gateway
- the IP address on the **local network** that packets are forwarded to when the destination is **not within the same network**
	- When a device needs to communicate with a device outside its own subnet, the data will be sent to the gateway
	- acts as the exit point for any data leaving the local network
#### IPv4
- Dotted-decimal notation 4 * 0-255
	- each number is a 8-bit binary number
- `133.33.3.7`
	- network part (`133.33`)
		- states which IP network this IP address belongs to
	- host part (`3.7`)
		- represents hosts on the network
		- `3.7` in this case is one device in the network (ex. a laptop)
- If the "Network" part of 2 IP addresses match, they're on the same IP network
	- if match -> devices are local
	- if not match -> devices are remote
	- using submasks
#### IPv6
# Routing
![[routing.png]]
- The routing layer
	- Layer that routers use to determine how to forward traffic
	- It **figures out the path** data should take across different networks.
	- It uses **IP (Internet Protocol) addresses** (not MAC addresses anymore).
- When the packets are leaving, it's forwarding it at [[Layer 2 - Data Link|layer 2]], it's packed in a frame
	- in this ex) the frame has the AWS's MAC address as its destination
#### Route Tables & Routes
- **Routing table**:
	- A collection of **routes** used to determine where to forward IP packets
- **Packet forwarding** process
	- When a packet arrives, the router checks the **destination IP**
    - It looks for **matching routes** in the routing table
    - If **multiple routes match**, the router chooses the **most specific route** (i.e., the one with the **longest prefix**)
- **Route specificity**
    - The **higher the number after the slash**, the **more specific** the route.
        - `/x` -> first x bits are for the network
        - `/0` is the least specific (matches **everything** — "default route")
        - `/32` is the most specific (matches **only one IP**)
- default route
	- `0.0.0.0/0`

# Address Resolution Protocol (ARP)
>[!arp]
>ARP is used to map a **Layer 3 IP address** to a **Layer 2 MAC address**
>- When a device wants to send a packet to another IP on the **same local network**, it needs to encapsulate the packet in a frame
>- But to send a frame, it must know the **destination MAC address**.
>- If it doesn't know the MAC address for a given IP, it uses **ARP** to find it.
- between layer 2 and 3
![[arp.png]]

# Fragmentation
- if needed, layer 3 breaks packets into smaller pieces (fragments) so it can travel across networks that have size limits
- Fragments frames to traverse different networks

# Example - Routing
![[example-routing.png]]
