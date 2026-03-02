
>[!physical]
>Layer 1 (Physical) specifications define the **transmission and reception** of RAW BIT STREAMS between a deice and a **SHARED** physical medium
>- simply transmits any data it receives onto the physical medium
>- defines things like voltage lvls, timing, rates, distances, modulation, and connectors
>- both devices use this physical medium to send and receive raw data
- The physics of the network
	- signaling, cabling, connectors
	- physical medium can be copper (electrical), fibre (light) or WIFI (RF)
	- No concept of protocols, addresses, or error handling.
		- Operates purely on physics
- 1 broadcast and 1 collision domain
	- not scaled very well, the more devices, the higher chance of collisions and data corruption
- for this to be useful, we need to add layer 2 which works on top of a working layer 1 connection
## Responsibilities
- Signaling: Voltage levels, light pulses, RF signals.
- Cabling: Copper, fiber, wireless.
- Connectors: RJ-45, LC, etc.
- Modulation: How bits are represented physically.
## Common Issues ("Physical Layer Problems")
- Bad cabling/punch-downs, faulty connectors/adapters
- Fix: Loopback tests, cable replacement, NIC swaps

## Examples
#### Direct Connection (Point-to-Point)
- diagram
	![[osi_physical_ex1.png]]
- connecting 2 laptops together in a LAN with a copper network cable, carried unstructured information
- a point-to-point electrical shared medium between these 2 devices (between the 2 network interface cards)
#### Hub (Multi-Device Shared Medium)
- diagram
	![[osi_physical_hub.png]]
	- HUB with 4 ports.
	- No addresses (Layer 2 needed for that).
- Collisions are a problem
		![[layer1-problem-data-collision.png]]
	- One device transmits at a time; otherwise, collisions.
	- Everything received on one port is retransmitted to all others (including errors/collisions).
	- collisions are guaranteed, and it cannot even detect when collisions occur, as this layer just transmits voltage via shared medium