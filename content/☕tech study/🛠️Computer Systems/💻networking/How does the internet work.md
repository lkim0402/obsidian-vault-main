- How does the internet work? (Full Course) - https://youtu.be/zN8YNNHcaZc?si=raWL7_Fm0ZNNncFd
## What is the switch?
![[Pasted image 20250212153156.png|500]]
- The switch connects the computers and allows them to communicate
- It creates a LAN and it has LAN ports for the computers to connect 
	- connected using wires, typically Cat5 or Cat6 cable
	- *local area network (LAN) =  a group of devices connected within a physically close area. A home WiFi network is a common example of a LAN.
- The switch looks at the packet (the msg) and sees where it should be headed, then sends the packet accordingly 
- The computers so far can only communicate with themselves and not with the internet
- Campus Area Network (CAN)
	- You can also create a LAN by CONNECTING SWITCHES (if the distance is close between the switches)![[Pasted image 20250213131126.png|500]]
## What is the router?
#### Used to connect to the internet
- A router is required to connect a computer/network (like LANs) to the internet (enables computers to connect to internet)
	![[Pasted image 20250212154157.png|500]]
- The connection between the router and the internet 
	- the cable is given to us by the ISP (Internet Service Provider)
- If a computer can send packets to the internet, this means that this computer can connect to the internet w/o any problem
	- The router receives the packet and sends it to the internet
#### Used to connect different networks
![[Pasted image 20250213135217.png|500]]
- 2 different LANs are connected through a router
- commonly different networks are used in different offices irl
## Internet Service Provider (ISP)
> Companies that enable us to connect to the internet (for money)
![[Pasted image 20250213135710.png|500]]
- Responsible for packets from one location to another
- Each ISP is responsible for specific routers
- The 1st step to connecting to the internet is the local ISP, which are responsible for small area communications
	- If a PC in a village wants to connect to another PC in the same village, it will directly use the local ISP 
	- every home/office must purchase a cable to connect your router to the internet 
	- some local ISP can connect directly to the global ISP
- The routers represent POPs (Point of prescence)
	- a physical location where networks and communication devices connect
	- There are  many POPs distributed and routers are *in* this POP
- Local ISPs connect neighborhoods and Regional ISPs connect cities
- Network of a country = all local ISPs + regional ISPs
- You don't have to connect to a local ISP to connect to the internet -> you can directly connect to a regional/global ISP -> it's your decision to choose the provider you want!
## The Wide Area Network (WAN)
- network consisting of different LANs
- companies that has different LANs across the globe might want a WAN instead of just using the Internet for information security reasons
#### WAN + VPN
- can set up a WAN using a VPN (virtual private network)
	- ensures our anonymity and encrypts our data before sending the packet
	- The tunneling feature of the VPN provides privacy and anonymity and security![[Pasted image 20250212201632.png]]
		- creates a special network connection over a public network
		- not a physical tunnel ofc, the tunnel is a high security for the connection bet. the 2 routers -> encryption + encapsulation (when packet is received, it goes through decapsulation and decryption)
	- Site to Site VPN
- (WAN + VPN) or LAN? Which is more secure?
	- A LAN is always more secure because the packet never moves through the internet
#### Private WAN
- different with wan + vpn
- ISP gives you a private line ONLY your company can use
	![[Pasted image 20250213134704.png|500]]
- Private WAN can be quite costly exp over long distances
## The Internet!
![[Pasted image 20250212154749.png|500]]
- Many routers in the internet!
	- since routers connect with other networks/computers, connecting to the internet can also stand for connecting to any other computer in the world
- Home router
	- The LANs have 1 home router -> switch + router
	- Most home routers nowadays also have an access point feature (enables wireless technology)
	- enough for small environments (home, office, etc)
- If you use 1 giant router for the internet instead of many (like the diagram)
	- Single point of failure (the router will be overloaded)
	- huge mess in connecting all computers in the world to a single router with cables
	- if some router fails, efficiency decreases only a lil bit (if its a giant one, if it fails, then all fails)
- Each router has a special processor that create a "Routing Table" using algorithms
	- The router looks this table up when it receives a package to see which router it should forward the package
- A router always wants to deliver the packet to the destination ASAP
- each router in the diagram actually represents a POP 

### With servers
![[Pasted image 20250213142822.png|500]]
- After the server of the website receives the request message, it sends a response msg (info about the webpage (img, vids, html files, and everything else))
- For big comapnies like google, its servers are distributed around the world
	- distributed server structure
- Peering
	- a direct connection established between two separate networks, allowing them to exchange traffic with each other directly without going through a third-party provider

## Other NOTES
- The internet is a structure that connects ALL LANs and computers in the WORLD
	- Massive undersea cables are connecting all continents on Earth
	- https://www.submarinecablemap.com/
	- Each cable is HUGE, consisting of 100s of optic fibers, each using lasers to transmit up to 400 GB of data per second
	- Tiny electric signals travel at the speed of light through oceans
- Every single computer/device that's connected to the Internet has an IP address
- A server is a computer that's directly connected to the internet, and webpages are files stored on that hard drive
	- ex: `udemy.com` has many diff servers around the globe
	![[Pasted image 20250207154605.png|400]]
- Every server has a unique IP (Internet Protocol) address
	![[Pasted image 20250207154701.png]]
	- But we use domain names instead of IP addresses
- Your computer at home is NOT a server, because it's not connected directly to the Internet. They're called clients because they're connected indirectly through an ISP 
	- ISP are the people you pay to be able to access the internet
	- ISP gives the IP address of your device

- When client makes a request to the web browser `google.com`
	- Client -> ISP -> Domain Name System (DNS) -> server -> data is sent
	- Browser sends the message `google.com` to your ISP
	- ISP will relay the msg to the DNS server
		- DNS server looks up its database to find the exact IP address of that website you're trying to access
	- DNS server finds that IP address and gives to browser
	- Browser sends that IP address to the server and fetch the necessary data back to your browser 
	- Data flow starts, transferred in digital format via optical fiber cables
	- Data (light pulse) flows into the router where the cables are connected, and are changed into electric signals, then an ethernet cable if it's connected to your laptop
		- But if you're using cellular data, the light pulse is sent to a cell tower, then the signal reaches your device in electromagnetic waves