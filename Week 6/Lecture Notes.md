# Network Access Layer

### Introduction to Networks and Operating Systems
These notes provides an overview of week 6's networks and operating systems, setting the stage for the more detailed topics covered later.

### Application Layer
The application layer is the top layer of the TCP/IP model. It is responsible for providing network services to applications.

### Transport Layer - 1 & 2
The transport layer provides reliable and unreliable communication between applications. It uses protocols such as TCP and UDP.

### Internet Layer
The internet layer is responsible for routing data packets between networks. The main protocol used here is IP (Internet Protocol).

### Network Access Layer
This layer handles the physical transfer of data between two nodes on a network. It covers topics like error detection, multiple access protocols, and LAN technologies such as Ethernet.

### Link Layer: Introduction
> Terminology:
>   - Hosts and routers: Nodes.
>   - Communication channels that connect adjacent nodes along communication path: links. * Wired * Wireless * LANs
- Layer-2 packet: Frame, encapsulates datagram.
- Focuses on transferring datagrams between physically adjacent nodes over a link.
- Example networks: Mobile, enterprise, ISP, data center.

### Link Layer: Services
- Framing, link access:
  - Encapsulating datagrams into frames (adding header and trailer).
  - Channel access in shared mediums.
  - MAC addresses in frame headers for source and destination.
- Reliable delivery between adjacent nodes.
  - Not always used on low bit-error links.
  - Wireless links often require reliability due to high error rates.
- Flow Control: Pacing between adjacent sending and receiving nodes.
- Error Detection: Detecting errors caused by signal attenuation and noise. Receivers can request retransmission or drop frames.
- Error Correction: Correcting bit errors without retransmission.
- Duplex Communication:
  - Half-duplex: Nodes can transmit but not simultaneously.
  - Full-duplex: Nodes can transmit simultaneously.

### Where is the link layer implemented?
- Implemented in each host.
- Typically within the Network Interface Card (NIC) or on a chip (e.g., Ethernet, WiFi card).
- Combination of hardware, software, and firmware.
- Attaches to the host's system bus (e.g., PCI).

### Sending and Receiving Sides
- Sending Side:
  - Encapsulates datagram in a frame.
  - Adds error checking bits, reliability mechanisms, and flow control.
- Receiving Side:
  - Checks for errors, manages reliable data transfer, and flow control.
  - Extracts datagram and passes it to the upper layer.

### Error Detection & Correction
- Error Detection
- EDC (Error Detection and Correction) bits: Redundancy bits added to the data.
- D: Data protected by error checking (may include header fields).
- Error detection isn't always 100% reliable.
- Larger EDC fields improve error detection and correction capabilities.
The process:
- Sender calculates EDC bits based on data D and sends D and EDC.
- Receiver receives D' and EDC'.
- Receiver checks if all bits in D' are OK using EDC'.
- If errors are detected, the frame is dropped or retransmission is requested.

### Parity Checking
- Single Bit Parity: Detects single bit errors.
  - Add a parity bit to make the number of 1s either even (even parity) or odd (odd parity).
- Two-Dimensional Bit Parity: Detects and corrects single bit errors.
Example:
Consider the following data:
![image](https://github.com/user-attachments/assets/97589ab8-622e-401a-bfce-dd74111b8978)
Row parity is calculated for each row and column parity for each column.
- Example of single-bit error detection and correction using row and column parity.
I
### nternet Checksum (Review)
- Sender:
  - Treats the contents of the UDP segment (including header fields and IP addresses) as a sequence of 16-bit integers.
  - Checksum: One's complement sum of the segment content.
  - The checksum value is placed in the UDP checksum field.
- Receiver:
  - Computes the checksum of the received segment.
- Checks if the computed checksum equals the checksum field value.
  - If not equal, an error is detected.
  - If equal, no error is detected (but errors may still exist).

Goal: Detect flipped bits in a transmitted segment.

### Cyclic Redundancy Check (CRC)
- More powerful error-detection coding.
- D: Data bits (treated as a binary number).
- G: Generator bit pattern (r+1 bits).
- R: CRC bits.
The process:
- The sender has data D and a generator G.
- The sender calculates R such that D concatenated with R is divisible by G using binary long division.
- The sender transmits D and R.
- The receiver divides the received bit stream by G. If the remainder is zero, there are no errors; otherwise, an error is detected.

## Multiple Access Protocols

### Multiple Access Links, Protocols
- Point-to-Point Links: Direct link between two nodes (e.g., Ethernet switch and host, PPP for dial-up).
- Broadcast Links (Shared Wire or Medium): Multiple nodes share the same communication medium (e.g., old Ethernet, upstream HFC in cable networks, 802.11 wireless LANs).
Examples of Shared Media:
- Shared wire (cabled Ethernet)
- Shared radio (WiFi, satellite, 4G/5G)
# Multiple Access Protocols
- Challenge: How to share a single broadcast channel among multiple nodes.
- Simultaneous transmissions cause interference (collision).
- Distributed Algorithm: Determines how nodes share the channel (when a node can transmit).
- Communication about channel sharing must use the channel itself.
Ideal Multiple Access Protocol
- Given: Multiple access channel (MAC) of rate R bps.
- When one node wants to transmit, it can send at rate R.
When  M nodes want to transmit, each can send at an average rate of R/M.
- Fully decentralized:
  - No special node to coordinate transmissions.
  - No synchronization of clocks, slots.
- Simple.
### Taxonomy of MAC Protocols
- Channel Partitioning: Divides the channel into smaller pieces (time slots, frequency, code) and allocates pieces to nodes for exclusive use.
- Random Access: Channel is not divided; collisions are allowed, and nodes recover from collisions.
- Taking Turns: Nodes take turns, but nodes with more to send can take longer turns.
### Channel Partitioning MAC Protocols
TDMA (Time Division Multiple Access)
- Access to the channel in "rounds."
- Each station gets a fixed-length slot in each round.
- Unused slots go idle.
- Example: 6-station LAN; stations 1, 3, and 4 have packets to send; slots 2, 5, and 6 are idle.
FDMA (Frequency Division Multiple Access)
- Channel spectrum divided into frequency bands.
- Each station assigned a fixed frequency band.
- Unused transmission time in frequency bands goes idle.
- Example: 6-station LAN; stations 1, 3, and 4 have packets to send; frequency bands 2, 5, and 6 are idle.

### Random Access Protocols
- When a node has a packet to send, it transmits at full channel data rate R.
- No prior coordination among nodes.
- Two or more transmitting nodes: "collision."
- Random access MAC protocol specifies:
  - How to detect collisions.
  - How to recover from collisions (e.g., via delayed retransmissions).
Examples: ALOHA, slotted ALOHA, CSMA, CSMA/CD, CSMA/CA.
Pure ALOHA
- Unslotted ALOHA: simpler, no synchronization.
- When a frame arrives, transmit immediately.
- Collision probability increases due to lack of synchronization.
- Frame sent at time t0 collides with other frames sent in [t0 - 1, t0 + 1]
- Pure ALOHA efficiency: 18%!
Slotted ALOHA
Assumptions:
- All frames are the same size.
- Time is divided into equal-size slots (time to transmit 1 frame).
- Nodes start transmitting only at the beginning of a slot.
- Nodes are synchronized.
- If 2 or more nodes transmit in a slot, all nodes detect a collision.
Operation:
- When a node obtains a fresh frame, it transmits in the next slot.
- If no collision, the node can send a new frame in the next slot.
- If a collision occurs, the node retransmits the frame in each subsequent slot with probability p until success.
Pros:
- A single active node can continuously transmit at the full rate of the channel.
- Highly decentralized: only slots need to be synchronized.
- Simple.
Cons:
- Collisions waste slots.
-Idle slots.
- Nodes may be able to detect collisions in less time than it takes to transmit a packet.
- Requires clock synchronization.
Efficiency: Long-run fraction of successful slots (many nodes, all with many frames to send).
Suppose:
- N nodes with many frames to send, each transmits in a slot with probability p.
- The probability that a given node has success in a slot is p(1-p)^N-1.
- The probability that any node has a success is Np(1-p)^N-1.
- Max efficiency: find p* that maximizes Np(1-p)^N.
For many nodes, take the limit as N goes to infinity:
![image](https://github.com/user-attachments/assets/72b22c6d-6d9e-4096-bc7d-4e2e60ed6428)

At best: the channel is used for useful transmissions 37% of the time!
CSMA (Carrier Sense Multiple Access)
Simple CSMA: Listen before transmit:
- If the channel is sensed idle, transmit the entire frame.
- If the channel is sensed busy, defer transmission.
- Analogy: Don't interrupt others!
CSMA/CD (CSMA with Collision Detection)
- Collisions are detected within a short time.
- Colliding transmissions are aborted, reducing channel wastage.
- Collision detection is easy in wired networks, difficult in wireless.
- Analogy: The polite conversationalist.
Collisions:
- Collisions can still occur with carrier sensing:
  - Propagation delay means two nodes may not hear each other's just-started transmission.
- Collision: The entire packet transmission time is wasted.
- Distance & propagation delay play a role in determining collision probability.
Ethernet CSMA/CD Algorithm
- The NIC receives a datagram from the network layer and creates a frame.
- If the NIC senses the channel:
  - If idle: start frame transmission.
  - If busy: wait until the channel is idle, then transmit.
- If the NIC transmits the entire frame without collision, the NIC is done with the frame!
- If the NIC detects another transmission while sending: abort, send a jam signal.
- After aborting, the NIC enters binary (exponential) backoff:
![image](https://github.com/user-attachments/assets/c3580cf6-a19f-4886-98d7-1d69c346fcc0)
Better performance than ALOHA: Simple, cheap, and decentralized!

### "Taking Turns" MAC Protocols
- Channel partitioning MAC protocols:
  - Share the channel efficiently and fairly at high load.
  - Inefficient at low load: delay in channel access, 1/N bandwidth allocated even if only 1 active node!
- Random access MAC protocols:
  - Efficient at low load: a single node can fully utilize the channel.
  - High load: collision overhead.
- "Taking turns" protocols:
  - Look for the best of both worlds!
Polling
- A master node "invites" other nodes to transmit in turn.
- Typically used with "dumb" devices.
- Concerns:
  - Polling overhead.
  - Latency.
  - Single point of failure (master).
Token Passing
- A control token is passed from one node to the next sequentially.
- Concerns:
  - Token overhead.
  - Latency.
  - Single point of failure (token).

### Cable Access Network: FDM, TDM, and Random Access!
- Cable headend CMTS (cable modem termination system).
- Frames, TV channels, control are transmitted downstream at different frequencies.
- Multiple downstream (broadcast) FDM channels: up to 1.6 Gbps/channel.
- Single CMTS transmits into channels.
- Multiple upstream channels (up to 1 Gbps/channel).
- Multiple access: all users contend (random access) for certain upstream channel time slots; others assigned TDM.
DOCSIS (Data Over Cable Service Interface Specification):
- FDM over upstream, downstream frequency channels.
- TDM upstream: some slots assigned, some have contention.
  - Downstream MAP frame: assigns upstream slots.
  - Requests for upstream slots (and data) transmitted using random access (binary backoff) in selected slots.
### Summary of MAC Protocols
- Channel partitioning, by time, frequency, or code (Time Division, Frequency Division).
- Random access (dynamic): ALOHA, S-ALOHA, CSMA, CSMA/CD.
  - Carrier sensing: easy in some technologies (wire), hard in others (wireless).
  - CSMA/CD used in Ethernet.
  - CSMA/CA used in 802.11.
- Taking turns: polling from central site, token passing (Bluetooth, FDDI, token ring).

## LANs

### MAC Addresses
- 32-bit IP address: Network-layer address for an interface. Used for layer 3 (network layer) forwarding.
  - e.g.: 128.119.40.136
- MAC (or LAN or physical or Ethernet) address: Used “locally” to get a frame from one interface to another physically-connected interface (same subnet, in IP-addressing sense).
  - 48-bit MAC address (for most LANs) burned in NIC ROM, also sometimes software settable.
  - Hexadecimal (base 16) notation (each "numeral" represents 4 bits).
  - e.g.: 1A-2F-BB-76-09-AD
Each interface on the LAN:
- Has a unique 48-bit MAC address.
- Has a locally unique 32-bit IP address.
MAC Address Allocation
- MAC address allocation is administered by IEEE.
- Manufacturers buy portions of MAC address space (to assure uniqueness).
- Analogy:
  - MAC address: like National Insurance Number.
  - IP address: like postal address.
- MAC flat address: portability
  - Can move the interface from one LAN to another.
  - IP address is not portable: depends on the IP subnet to which the node is attached.

### ARP: Address Resolution Protocol
ARP table: Each IP node (host, router) on a LAN has a table. Question: How to determine the interface's MAC address, knowing its IP address?

ARP Table Structure: `<IP address; MAC address; TTL>`
- TTL (Time To Live): Time after which the address mapping will be forgotten (typically 20 min).
ARP Protocol in Action Example: A wants to send a datagram to B.
- B's MAC address is not in A's ARP table, so A uses ARP to find B's MAC address.
- A broadcasts an ARP query, containing B's IP address.
- The destination MAC address = FF-FF-FF-FF-FF-FF.
- All nodes on the LAN receive the ARP query.
- B replies to A with an ARP response, giving its MAC address.
- A receives B’s reply, adds B’s entry into its local ARP table.

### Routing to Another Subnet: Addressing Walkthrough
> Scenario: Sending a datagram from A to B via R.
- Focus on addressing – at IP (datagram) and MAC layer (frame) levels.
Assumptions:
- A knows B’s IP address.
- A knows the IP address of the first hop router, R (how?).
- A knows R’s MAC address (how?).
Steps:
- A creates an IP datagram with the IP source of A and destination of B.
- A creates a link-layer frame containing A-to-B IP datagram.
  - R's MAC address is the frame’s destination MAC.
- The frame is sent from A to R.
- The frame is received at R; the datagram is removed and passed up to IP.
- R determines the outgoing interface, passes the datagram with the IP source of A and destination of B to the link layer.
- R creates a link-layer frame containing A-to-B IP datagram.
  - Frame destination address: B's MAC address.
- Transmits link-layer frame.
- B receives the frame, extracts the IP datagram destined for B.
- B passes the datagram up the protocol stack to IP.

## Ethernet

### Ethernet
- "Dominant" wired LAN technology:

- First widely used LAN technology.
- Simpler, cheaper.
- Kept up with speed race: 10 Mbps – 400 Gbps.
- Single chip, multiple speeds (e.g., Broadcom BCM5761).

### Ethernet: Physical Topology
- Bus: Popular through mid-90s
  - All nodes in the same collision domain (can collide with each other).
- Switched: Prevalent today
  - An active link-layer 2 switch in the center.
  - Each “spoke” runs a (separate) Ethernet protocol (nodes do not collide with each other).

### Ethernet Frame Structure
A sending interface encapsulates an IP datagram (or other network-layer protocol packet) in an Ethernet frame:
`| Preamble | Dest. Address | Source Address | Type | Data (Payload) | CRC |`
- Preamble: Used to synchronize receiver and sender clock rates.
  - 7 bytes of 10101010 followed by one byte of 10101011.
- Addresses: 6-byte source and destination MAC addresses.
  - If the adapter receives a frame with a matching destination address or a broadcast address (e.g., ARP packet), it passes the data in the frame to the network-layer protocol.
  - Otherwise, the adapter discards the frame.
- Type: Indicates a higher-layer protocol
  - Mostly IP, but others are possible, e.g., Novell IPX, AppleTalk.
  - Used to demultiplex up at the receiver.
- CRC (Cyclic Redundancy Check): Used for error detection at the receiver.
  - If an error is detected, the frame is dropped.

### Ethernet: Unreliable, Connectionless
- Connectionless: No handshaking between sending and receiving NICs.
- Unreliable: The receiving NIC doesn't send ACKs or NAKs to the sending NIC.
  - Data in dropped frames is recovered only if the initial sender uses a higher-layer RDT (e.g., TCP); otherwise, dropped data is lost.
- Ethernet's MAC protocol: Unslotted CSMA/CD with binary backoff.

### 802.3 Ethernet Standards: Link & Physical Layers
- Different physical layer media: fiber, cable
- Many different Ethernet standards:
  - Common MAC protocol and frame format.
  - Different speeds: 2 Mbps, 10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps, 40 Gbps.

## Switches
- Ethernet Switch
- Switch is a link-layer device that takes an active role:
  - Stores and forwards Ethernet frames.
  - Examines the incoming frame’s MAC address and selectively forwards the frame to one or more outgoing links.
  - When the frame is to be forwarded on a segment, it uses CSMA/CD to access the segment.
- Transparent: hosts are unaware of the presence of switches.
- Plug-and-play, self-learning.
  - Switches do not need to be configured.

### Switch: Multiple Simultaneous Transmissions
- Hosts have a dedicated, direct connection to the switch.
- Switches buffer packets.
- The Ethernet protocol is used on each incoming link, so:
  - No collisions; full duplex.
  - Each link is its own collision domain.
- Switching: A-to-A' and B-to-B' can transmit simultaneously, without collisions.
- But A-to-A' and C-to-A' cannot happen simultaneously.

### Switch Forwarding Table
Q: How does the switch know A' is reachable via interface 4, B' is reachable via interface 5? A: Each switch has a switch table, each entry:
- (MAC address of host, interface to reach host, timestamp).
- Looks like a routing table!
Q: How are entries created and maintained in the switch table?
- Something like a routing protocol?

### Switch: Self-Learning
- The switch learns which hosts can be reached through which interfaces.
- When a frame is received, the switch “learns” the location of the sender: incoming LAN segment.
- It records the sender/location pair in the switch table.

### Switch: Frame Filtering/Forwarding
When a frame is received at the switch:
- Record the incoming link and MAC address of the sending host.
- Index the switch table using the MAC destination address.
- If an entry is found for the destination, then:
  - If the destination is on the segment from which the frame arrived, drop the frame.
  - Else, forward the frame on the interface indicated by the entry.
- Else, flood (forward on all interfaces except the arriving interface).

### Self-Learning, Forwarding: Example
- Frame destination, A', location unknown: flood.
- Destination A location known: selectively send on just one link.

### Interconnecting Switches
Self-learning switches can be connected together. Q: Sending from A to G - how does S1 know to forward frame destined to G via S4 and S3?
- A: self-learning! (works exactly the same as in the single-switch case!)

### Small Institutional Network
An example network including mail servers, web servers, and routers, all connected using switches.

### Switches vs. Routers
Both are store-and-forward:
- Routers: Network-layer devices (examine network-layer headers).
- Switches: Link-layer devices (examine link-layer headers).
Both have forwarding tables:
- Routers: Compute tables using routing algorithms, IP addresses.
- Switches: Learn forwarding tables using flooding, learning, MAC addresses.

## Overall Network Operations
### Synthesis: A Day in the Life of a Web Request
Goal: Identify, review, and understand protocols (at all layers) involved in a seemingly simple scenario: requesting a WWW page. Scenario: A student attaches a laptop to the campus network and requests/receives www.google.com.

### A Day in the Life: Scenario
- A mobile client arrives and attaches to the network...
- ...and requests a web page: www.google.com.

### A Day in the Life: Connecting to the Internet
- The router has a DHCP server.
- The arriving mobile client: DHCP client.
- Connecting the laptop needs to get its own IP address, address of the first-hop router, address of the DNS server: use DHCP.
- DHCP request is encapsulated in UDP, encapsulated in IP, encapsulated in 802.3 Ethernet.
- The Ethernet frame is broadcast (dest: FFFFFFFFFFFF) on the LAN and received at the router running the DHCP server.
- Ethernet is demuxed to IP demuxed, UDP demuxed to DHCP.
- The DHCP server formulates a DHCP ACK containing the client's IP address, IP address of the first-hop router for the client, and name & IP address of the DNS server.
- Encapsulation at the DHCP server, frame forwarded (switch learning) through LAN, demultiplexing at the client.
- Client now has an IP address and knows the name & address of the DNS server, and the IP address of its first-hop router.
- The DHCP client receives the DHCP ACK reply.

### A Day in the Life: ARP (before DNS, before HTTP)
- Before sending the HTTP request, need the IP address of www.google.com: DNS.
- A DNS query is created, encapsulated in UDP, encapsulated in IP, encapsulated in Ethernet.
- To send the frame to the router, need the MAC address of the router interface: ARP.
- An ARP query is broadcast and received by the router, which replies with an ARP reply giving the MAC address of the router interface.
- The client now knows the MAC address of the first hop router, so can now send the frame containing the DNS query.

### A Day in the Life: Using DNS
- The IP datagram containing the DNS query is forwarded via the LAN switch from the client to the 1st hop router.
- The IP datagram is forwarded from the campus network into the Comcast network, routed (tables created by RIP, OSPF, IS-IS, and/or BGP routing protocols) to the DNS server.
- Demuxed to DNS.
- DNS replies to the client with the IP address of www.google.com.

### A Day in the Life: TCP Connection Carrying HTTP
- To send an HTTP request, the client first opens a TCP socket to the web server.
- A TCP SYN segment (step 1 in the TCP 3-way handshake) is inter-domain routed to the web server.
- The TCP connection is established!
- The web server responds with a TCP SYNACK (step 2 in the TCP 3-way handshake).

### A Day in the Life: HTTP Request/Reply
- The HTTP request is sent into the TCP socket.
- The IP datagram containing the HTTP request is routed to www.google.com.
- The IP datagram containing the HTTP reply is routed back to the client.
- The web server responds with an HTTP reply (containing the web page).
= The web page is finally (!!!) displayed.

## Summary
- Principles behind data link layer services:
  - Error detection and correction.
  - Sharing a broadcast channel: multiple access.
Link-layer addressing.
- Instantiation and implementation of various link layer technologies:
  - Ethernet.
  - Switched LANs, VLANs.
  - Virtualized networks as a link layer: MPLS.
- Synthesis: a day in the life of a web request.
