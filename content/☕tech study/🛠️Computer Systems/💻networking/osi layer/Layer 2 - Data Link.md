
>[!Data link]
>The most fundamental layer used for communication between 2 devices locally on the network
>- The most critical layer in the entire OSI 7 layer model
>- layer 2 runs on top of different types of [[Layer 1 - Physical|layer 1]] networks (copper, fiber, wifi)
>	- supports the transfer of the data
>- the [[Layer 2 - Data Link#Layer 2 using a Switch|switching layer]]

- The basic network "language"
	- The foundation of communication at the data link layer
- Layer 2 provides frames, and layer 1 handles the physical transmission / reception between the shared medium
	- layer 2 sends the frame onto the physical medium, layer 1 doesn't understand the frame but transmits the raw data onto the medium
- Encapsulation
	- the process of **taking data from upper layers (like Layer 3)** and **wrapping it inside a Layer 2 frame**
- Decapsulation 
	- happens when a Layer 2 device **receives a frame** and **removes the Layer 2 header/trailer** to **extract the data**
	- The frame is **stripped off**, and the **payload is passed up** to the next layer (Layer 3 — Network layer)
- For anything using Layer 2 to communicate, they see it as layer 2 on the left directly communicating with layer 2 on the right even tho layer 1 is used
	- Anything using layer 2 services has no visibility of layer 1
	- common in the OSI model (anything below the point you're communicating with is abstracted away)
## Concepts
![[layer2_diagram.png]]
#### Frames
>[!frame]
>A format for sending information over a layer 2 network. 
>Layer 2 uses this for communication.
- a container
	- preamble (start frame delimiter)
		- allow devices to know that it's the start of the frame
	- MAC header
		- source - the device address from whatever is transmitting the frame
		- destination - u can put all Fs if you want to broadcast (send to every device)
		- EtherType (ET field)
			- which layer 3 protocol originally put data into the frame
			- ex) IP (internet protocol)
				- IPv4 - `0x0800`
				- IPv6 - `0x86DD`
			- which layer 3 protocol receives the data at the destination
	- Payload
		- 46-1500 bytes
		- data frame is sending (data provided by the [[Layer 3 - Network|layer 3]] protocol - the [[Layer 3 - Network#IP Packets|IP Packet]])
		- the protocol used is mentioned in the ET field
	- Frame check sequence
		- Used to identity any errors in the frame
		- allows the destination to check if corruption occurred
####  MAC (Media Access Control) address
>[!definition]
>A globally unique identifier for every network interface (like a laptop’s Wi-Fi or Ethernet card)
>- a unique **hardware** address, NOT software assigned
>- Used to **identify devices** on a local network
- Formats
	- EUI-48/EUI-64
	- hexadecimal (48 bits long)
- Parts
	- OUI (Organizationally unique identifier)
		- assigned to companies who manufacture network devices
		- each companies have a separate OUI
	- Network Interface Controller (NIC specific)
	- Together, the MAC address on a network card should be globally unique
## Data link Control (DLC) protocols
- a general term for how Layer 2 controls communication
- Examples: Ethernet, Wi-Fi (IEEE 802.11), etc
## Communication & Encapsulation
- Previous problem with [[Layer 1 - Physical|layer 1]]
	![[layer1-problem-data-collision.png]]
#### Media Access Control - Carrier Sense Multiple Access (CSMA)
- Now we have MAC addresses and media control (we can check if there is a transmission already in layer 1) -> checks for a carrier
	![[layer2-data-link.png]]
	- it only sends data when career is none
- Types of CSMA
	- Collision Detection (CSMA/CD)
		- Used in **wired Ethernet** (especially old-school hubs), but not needed in modern switched Ethernet
		- If two devices transmit at the same time:
		    - A **collision** happens.
		    - Devices detect it and **stop**, **wait a random time**, then **try again**.
	- Collision Avoidance
		- Devices try to **avoid** collisions **before transmitting** by
		- Waiting random times (backoff)

#### Layer 2 using a HUB
- diagram
	![[layer2-hub.png]]
- When **Laptop 1** sends a frame intended for **Laptop 3**
	- the **hub** (a Layer 1 device) **forwards that frame to all ports**.
	- All connected devices receive the frame.
	- At **Layer 2**, each laptop's **network interface card (NIC)** checks the **destination MAC address** in the frame:
	    - If the MAC address **matches its own**, it **accepts** the frame.
	    - If not, it **discards** it.
- If 2 laptops sends data at exactly at the same time
	- A collision will happen on ALL ports of the hub 
	- The hub, being a **Layer 1 device**, has **no understanding of MAC addresses** or frame content.
    - It doesn't stop you from running a **Layer 2 protocol** on top of it, but it behaves purely at the **physical level**.
	- you should use switch
		- works same way physically as a hub
#### Layer 2 using a Switch
- diagram
	![[layer2-switch.png]]
- A switch works similarly to a hub **physically**, but it's a **Layer 2 device**.    
	- it has layer 2 software running inside it & understands layer 2
- It **reads MAC addresses** and **forwards frames only to the correct destination port**, **avoiding unnecessary traffic and collisions**.
- Switches store and forward frames
- Fills & learns the MAC Address table
	- Initially, it sends the frame to all the other ports
	- If it knows, then it will use that info
- Doesn't forward collisions
	- each port on the switch is a separate collision domain
	- No other devices share that link
	- If a problem occurs (like a misbehaving NIC), it’s **limited to that specific link** (Device A ↔ Port 1)
	- Other devices (B, C, etc.) aren’t affected because they have **their own separate links** with the switch
	- so if there is a collision, it will be limited to that one port only