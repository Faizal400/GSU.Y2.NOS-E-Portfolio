# Internet Layer - Revision Notes
## Introduction
> *These notes cover the Internet Layer within the TCP/IP model, based on the provided lecture slides. The Internet Layer is crucial for enabling communication across diverse networks.*

## 1. Internet Layer Overview
- The Internet Layer is responsible for routing data packets across networks.
- It uses IP addresses to identify the source and destination of data.

### TCP/IP Model Positioning
- Strategic Layer: Sits between the Transport and Network Access Layers.
- Core Responsibility: Enables logical, flexible communication across diverse networks.
- Analogy
> The Internet's "thin waist":
>   - One network layer protocol: IP
>   - Must be implemented by every (billions) of Internet-connected devices
>   - Many protocols in physical, link, transport, and application layers

### Mapping Concepts
- IP Address: Mailing address (e.g., 123 Main St., City, Post Code).
- Routers: Post offices (sort and forward packets/letters).
- Routing Protocols: Postal routes and logistics (optimizing delivery paths).
- Fragmentation: Splitting a large package into smaller boxes.
- ICMP Errors: "Return to sender" notifications.

### Core Responsibilities
- Addressing: Logical addressing for devices across networks (IP Addresses).
- Routing: Determining the path for data packets to reach their destination network.
- Packetization: Encapsulating Transport Layer segments into packets (IP Datagrams).
- Fragmentation & Reassembly: Handling packets that are too large for a network link.

### Key Functionalities
- Logical Addressing (IP Addresses): Independent of physical network.
- Global Network: Enabling communication across different types of networks.
- Connectionless Service: Best-effort delivery, no guarantees.
- Primary Protocol: IP (Internet Protocol) - IPv4, IPv6.

### Comparison with Transport Layer
![image](https://github.com/user-attachments/assets/aef044bd-ec78-46dc-8e7b-61a34caa8a98)

## 2. Internet Layer and Core Concepts
Core Concepts
- Routing:
  - Determines optimal paths for packets using routing tables and protocols (e.g., OSPF, BGP).
- Addressing:
  - Assigns IP addresses (IPv4/IPv6) to devices for identification.
- Fragmentation:
  - Splits large packets to fit MTU (Maximum Transmission Unit) of networks.
- Error Handling:
  - Detects and reports errors via ICMP (e.g., "Destination Unreachable").
### Data Plane
- Handles the actual movement of packets from a source to a destination.
- Forwards packets between interfaces based on control plane decisions.
Key Functions:
- Packet forwarding between routers and network interfaces.
- Applying policies like Quality of Service (QoS) or access control lists (ACLs).
- Ensuring minimal delay and efficient packet transmission.
Examples:
- A router inspecting the destination IP address of incoming packets and forwarding them to the appropriate output interface.
- The application of Network Address Translation (NAT) or packet filtering by a firewall.
### Control Plane
- Responsible for determining the routes that packets should take.
- Involves path computation and maintaining routing tables.
Key Functions:
- Routing protocol operations (e.g., OSPF, BGP, RIP) to discover and maintain routing paths.
- Network topology management and updates.
- Route selection and optimal path computation.
Examples:
- BGP (Border Gateway Protocol) exchanging routing information between different autonomous systems on the Internet.
- A router running OSPF to build and maintain its routing table dynamically.
- Handling ICMP messages to diagnose and troubleshoot network connectivity.

### Data Plane vs. Control Plane
![image](https://github.com/user-attachments/assets/465e95b7-b3c1-421e-a95e-5a4076bf12a9)

## 3. IP Addressing and Hierarchical Structure

### IPv4 Addressing
- 32-bit Structure: Split into 4 octets (e.g., 192.168.1.1).
Dotted-Decimal Notation: Conversion from binary (e.g., 11000000.10101000.00000001.00000001 → 192.168.1.1).
- Total Address Space: 4.3 billion addresses (2^32).

### IP Address Classes
- Class A (0.0.0.0–127.255.255.255): Large networks (16M hosts). Default mask: /8.
- Class B (128.0.0.0–191.255.255.255): Medium networks (65k hosts). Default mask: /16.
- Class C (192.0.0.0–223.255.255.255): Small networks (254 hosts). Default mask: /24.
- Class D (Multicast): 224.0.0.0–239.255.255.255.
- Class E (Experimental): 240.0.0.0–255.255.255.255.

### Special Addresses
- Private Addresses: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 (used in NAT).
- Loopback: 127.0.0.1 (self-communication).
- Broadcast: 255.255.255.255 (local network).
- Link-Local (APIPA): 169.254.0.0/16 (DHCP failure).

### CIDR (Classless Inter-Domain Routing)
- Subnet portion of address of arbitrary length.
- Address format: a.b.c.d/x, where x is # bits in subnet portion of address.
- Example: 200.23.16.0/23

```11001000 00010111 0001000 0 00000000
subnet part host part
```

### Address Acquisition
Two main questions:
- How does a host get an IP address within its network (host part of address)?
- How does a network get an IP address for itself (network part of address)?

### Host IP Address Acquisition
- Hard-coded: Manually configured by a system administrator (e.g., in /etc/rc.config on UNIX).
- DHCP (Dynamic Host Configuration Protocol): Dynamically obtains an address from a server.

### Network IP Address Acquisition
- A network gets an allocated portion of its provider ISP’s address space.
Example:
ISP's block: 200.23.16.0/20
The ISP can then allocate out its address space in smaller blocks, such as /23 blocks, to different organizations.

## 4. DHCP and Address Management

### DHCP Goal
- Host dynamically obtains an IP address from a network server when it “joins” the network.
- Can renew its lease on the address in use.
- Allows reuse of addresses (only hold address while connected/on).
- Supports mobile users who join/leave networks.

### DHCP Overview
- DHCP Discover: Host broadcasts a DHCP discover message.
- DHCP Offer: DHCP server responds with a DHCP offer message.
- DHCP Request: Host requests the offered IP address.
- DHCP ACK: DHCP server sends an acknowledgement, assigning the address.

### DHCP Message Exchange
- DHCP Discover:
  - Source: 0.0.0.0, 68
  - Destination: 255.255.255.255, 67
  - yiaddr: 0.0.0.0
  - Transaction ID: 654
- DHCP Offer:
  - Source: 223.1.2.5, 67
  - Destination: 255.255.255.255, 68
  - yiaddr: 223.1.2.4
  - Transaction ID: 654
  - Lifetime: 3600 secs
- DHCP Request:
  - Source: 0.0.0.0, 68
  - Destination: 255.255.255.255, 67
  - yiaddr: 223.1.2.4
  - Transaction ID: 655
  - Lifetime: 3600 secs
- DHCP ACK:
  - Source: 223.1.2.5, 67
  - Destination: 255.255.255.255, 68
  - yiaddr: 223.1.2.4
  - Transaction ID: 655
  - Lifetime: 3600 secs

### Additional DHCP Information
DHCP can return more than just an allocated IP address on the subnet:
- Address of the first-hop router for the client.
- Name and IP address of a DNS server.
- Network mask (indicating network versus host portion of address).

### DHCP Message Encapsulation
- DHCP REQUEST message encapsulated in UDP, encapsulated in IP, encapsulated in Ethernet.
- Ethernet frame broadcast (dest: FFFFFFFFFFFF) on LAN, received at router running DHCP server.
- Ethernet demuxed to IP demuxed, UDP demuxed to DHCP.
- DHCP server formulates DHCP ACK containing client’s IP address, IP address of the first-hop router for the client, name & IP address of DNS server.
- Encapsulated DHCP server reply forwarded to client, demuxing up to DHCP at client.
- Client now knows its IP address, name and IP address of DNS server, IP address of its first-hop router.

### Hierarchical Addressing Benefits
- Hierarchical addressing allows efficient advertisement of routing information.
Example: "Send me anything with addresses beginning 200.23.16.0/20"

### Address Allocation Authority
- ICANN (Internet Corporation for Assigned Names and Numbers): http://www.icann.org/
- Allocates IP addresses through 5 regional registries (RRs) (who may then allocate to local registries).
- Manages DNS root zone, including delegation of individual TLD (.com, .edu, …) management.

### IPv4 Address Exhaustion
- ICANN allocated the last chunk of IPv4 addresses to RRs in 2011.
- NAT (Network Address Translation) helps with IPv4 address space exhaustion.
- IPv6 has a 128-bit address space.

## 5. NAT and Subnets

### NAT (Network Address Translation)
- Allows multiple devices in a local network to share a single public IP address.
- All devices in the local network have 32-bit addresses in a "private" IP address space (10/8, 172.16/12, 192.168/16 prefixes) that can only be used in the local network.

### NAT Advantages
- Just one IP address needed from the provider ISP for all devices.
- Can change addresses of hosts in the local network without notifying the outside world.
- Can change ISP without changing addresses of devices in the local network.
- Security: Devices inside the local net are not directly addressable or visible by the outside world.

### NAT Implementation
NAT router must (transparently):
- Outgoing Datagrams: Replace (source IP address, port #) of every outgoing datagram to (NAT IP address, new port #).
  - Remote clients/servers will respond using (NAT IP address, new port #) as the destination address.
- NAT Translation Table: Remember (in NAT translation table) every (source IP address, port #) to (NAT IP address, new port #) translation pair.
- Incoming Datagrams: Replace (NAT IP address, new port #) in the destination fields of every incoming datagram with the corresponding (source IP address, port #) stored in the NAT table.

### NAT Operation Example
- Host 10.0.0.1 sends a datagram to 128.119.40.186, 80.
- NAT router changes the datagram source address from 10.0.0.1, 3345 to 138.76.29.7, 5001, and updates the NAT translation table.
- Reply arrives with the destination address: 138.76.29.7, 5001.
- NAT router uses the translation table to forward the datagram to 10.0.0.1, 3345.
### NAT Controversies
- Routers "should" only process up to layer 3.
- Address "shortage" should be solved by IPv6.
- Violates end-to-end argument (port # manipulation by network-layer device).
- NAT traversal: What if a client wants to connect to a server behind NAT?
### Subnets
- Definition: Device interfaces that can physically reach each other without passing through an intervening router.
- IP Addresses: Have structure:
  - Subnet part: Devices in the same subnet have common high-order bits.
  - Host part: Remaining low-order bits.

### Defining Subnets
- Detach each interface from its host or router, creating "islands" of isolated networks.
- Each isolated network is called a subnet.

### Subnet Mask
- Used to identify the subnet portion of an IP address (e.g., /24 means the high-order 24 bits are the subnet part of the IP address).

## 6. Routing and Forwarding
Key Functions
- Forwarding: Move packets from a router’s input link to the appropriate router output link. Analogy: Getting through a single interchange.
- Routing: Determine the route taken by packets from source to destination. Involves routing algorithms. Analogy: Planning a trip from source to destination.
Routing vs Forwarding
![image](https://github.com/user-attachments/assets/91887881-6730-4715-860b-51307639ff10)

### Routing Table Components
- Destination network/prefix
- Next hop address
- Exit interface
- Metric/cost
- Administrative distance

### Static Routing
- Also known as non-adaptive routing.
- The routing table doesn't change unless manually modified by the network administrator.
Advantages:
- Simple configuration
- Predictable behavior
- Low CPU/bandwidth overhead
- Enhanced security
Disadvantages:
- Manual configuration required
- No automatic adaptation to failures
- Not scalable for large networks

### Dynamic Routing
- Also known as adaptive routing.
- The routing table changes according to changes in the network topology.
Advantages:
- Automatic adaptation to network changes
- Scalable for large networks
- Load balancing capabilities
- Self-healing property
Disadvantages:
- More complex configuration
- Higher resource utilization
- Potential security risks
- Convergence time considerations

### Static vs Dynamic Routing
![image](https://github.com/user-attachments/assets/e56e05a0-e2cb-4515-b2dc-8f1bad69a336)

### Additional Metrics for Routing
- Hop Count: Number of routers between the source and destination. Simple but doesn't consider link quality.
- Latency: Time taken for packets to reach the destination. Important for real-time applications.
- Bandwidth: Available link capacity. Critical for high-throughput applications.
- Reliability
- Load
- MTU
- Cost

## 7. Routing Protocols

### RIP (Routing Information Protocol)
- One of the oldest distance-vector routing protocols.
- Employs the hop count as a routing metric.
- Prevents routing loops by implementing a limit on the number of hops allowed in a path from source to destination.
Implementation:
- Distance-vector protocol
- Maximum 15 hops
- Updates every 30 seconds
- Uses Bellman-Ford algorithm
- Simple configuration
- Suitable for small networks
- Limited scalability

### OSPF (Open Shortest Path First)
- A link-state routing protocol used to find the best path between the source and the destination router using its own Shortest Path First.
Characteristics:
- Link-state protocol
- Uses Dijkstra's algorithm
- Area-based hierarchy
- Fast convergence
Features:
- Support for VLSM (Variable Length Subnet Masking)
- Authentication
- Load balancing
- Incremental updates

### BGP (Border Gateway Protocol)
- A path-vector protocol designed to exchange routing and reachability information between autonomous systems (AS) on the Internet
Characteristics:
- Path-vector protocol
- Internet backbone protocol
- Policy-based routing
- AS path attributes
Key Concepts:
- Autonomous Systems
- Path attributes
- Route aggregation
- Policy enforcement

## 8. Advanced Routing Concepts

### Distance Vector Routing (DVR)
- A method used by routers to find the best path for data to travel across a network.
- Each router keeps a table that shows the shortest distance to every other router, based on the number of hops (or steps) needed to reach them.
- Routers share this information with their neighbors, allowing them to update their tables and find the most efficient routes.

### Dijkstra's Algorithm
- An algorithm for finding the shortest paths between nodes in a weighted graph, which may represent a road network.
- Developed by Edsger W. Dijkstra in 1956 and published three years later.

## 9. IPv6 and Network Evolution

### Motivation
- The initial motivation was that the 32-bit IPv4 address space would be completely allocated.
- Additional motivations include:
  - Speed processing/forwarding: 40-byte fixed length header.
  - Enable different network-layer treatment of “flows.”

### IPv6 Header
- Destination address (128 bits)
- Source address (128 bits)
- Payload len
- Next hdr
- Hop limit
- Flow label
- Pri
- Ver
Key Fields:
- Priority: Identify priority among datagrams in a flow.
- Flow label: Identify datagrams in the same "flow." (The concept of "flow" isn't well defined.)

### IPv6 Address
- 128-bit IPv6 addresses.

### IPv6 Differences from IPv4
- No checksum (to speed processing at routers).
- No fragmentation/reassembly.
- No options (available as upper-layer, next-header protocol at router).

### Transitioning from IPv4 to IPv6
- Not all routers can be upgraded simultaneously.
- No “flag days”.
  - How will the network operate with mixed IPv4 and IPv6 routers?

### Tunneling
- IPv6 datagram carried as payload in IPv4 datagram among IPv4 routers (“packet within a packet”).
- Tunneling is used extensively in other contexts (4G/5G).

### IPv6 Adoption
- Google: ~30% of clients access services via IPv6.
- NIST: 1/3 of all US government domains are IPv6 capable.

### Deployment Challenges
- Long (long!) time for deployment, use.
- 25 years and counting!
- Think of application-level changes in the last 25 years: WWW, social media, streaming media, gaming, telepresence, …
