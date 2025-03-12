# 🖥️ Week 1 - Introduction to Networks and Operating Systems 

## 1. Network Overview
## What is a Network?
> A system of links that interconnect nodes in order to move information between nodes.
> There are different types of networks, including:
- Internet
- Telephone network
- Cellular Network
- Supervisory control and data acquisition networks
- Optical networks
- Sensor networks
> Main focus is on the internet.

## The Internet's impact.
> Internet transformed:
- Buisness (E-Commerce, advertising, cloud computing)
- Relationships (social media, email etc)
- Learning (wikipedia, AI, search engines etc)
- Governance and Law (Censorship, copyright etc)

## Defining Characteristics of the internet
- Global connectivity: Connects people, devices, systems across the world
- Decentralisation: Not controlled by a single authority
- Interoperability: Enables different devices and systems to communicate through standard protocols(TCP,IP,HTTP, DNS)
- Scalability: Can expand without significant structural changes
- Accessibility: Accessible with simple technology (smartphone, computer)
- Multimedia support: Supports diverse types of media(text, images, audio, video)
- Hypertext & Hyperlinking: Enables easy navigaation through interconnected content.
- Real-Time Communication: Supports real-time interactions (instant messaging, video conferencing, live streaming)
- User-Driven Content: Users are active creators of content (social media, blogs, forums)
- Ubiquity & Mobility: Accessible anytime and anywhere via wireless networks and mobile devices
- Anonyimity & Privacy Challenges: Offers anonymity but also brings cybersecurity threats & privacy concerns

## The internet as a "Federated" System
- Tied together by IP (Internet Protocol), a single common interface.
- The internet ties together different networks
- >20,000 ISP Networks
- Interoperability is the internets most important goal

## Internet Scale (2020 numbers)
- 4.57 Billion users (roughly 58% of the worlds population)
- 1.8 Billion sites (34.5% in which are powered by WordPress)
- 4.88 Billion Smartphones (45.4% of population)
- 500 Million Tweets/day
- 1 Billion hours of YT Vids watched daily + 500 hours of YT Video added per/min
- 2+Bil Tiktok installs
- 60% video streaming
- 12.5% of the internet traffic is native Netflix

### Enormous Diversity & Dynamic Range
- Communication Latency: nanoseconds to seconds (10^-9)
- Bandwith: 100bits/second up to 400 Gigabits/second (10^9)
- Packet loss: 0-90%
- Tech: Optical, wireless, satelite etc
- Endpoint devices: from sensors to cellphones to datacenters and supercomputers
- Applications: social networking, file transfer, skype, live tv, gaming etc
- Users: the governing, goverend, malicious, addicted etc

### Constant Evolution
#### 1970s:
- 56 Kb (kilbit)/s backbone links
- <100 computers, handful in US and 1 in UK
- Telnet & file transfer are "killer" applications [?]
#### Today:
- 400+ Gb (gigabit)/s backbone links
- 40B+ devices all over the globe
- 27B+ IoT [?] devices alone

## The internet: Nuts & Bolts view
- Packet switches: forwards packets (chunks of data).
- Communcation links consists of fiber, copper, radio and satelite
- Networks are collection of devices, routers, links managed by an organization.

## Protocols are everywhere
- - Control sending, receiving of messages (HTTP for Web, streaming video, Skype, TCPIP, WiFi, 4G, Ethernet)
- - Internet standards:
  - - RFC: Request for Comments
    - IETF: Internet Engineering Task Force
## Internet: A service view
- Infastructure that provides services to applications:
--  Web, streaming video, multimedia, teleconfrencing, email, games, e-commerce, social media
- Provides programming interface to distributed applications:
- - "Hooks" allowing sneding/receiving apps to "connect" to use internet transport service
- Provides service options, analogous to postal service.

## What's a protocol?
Human protocols are things lke "Whats the time", or ensuring people are following rules in a workplace.
- Network protocols:
- Computers (devices) rather than humans)
- All communication activity in internet governed by protocols
Protocols define the format, order of messages, sent and received among network enteties, and actions taken on message transmission, receipt.

## Network Edge
- Hosts: Clients and servers
- Servers often in data centers
- Acess networks, physical media: wired, wireless communication links
- Network core: interconnected routers, network of networks

## Access to networks and physical media
- How to connect end systems to edge router?
- - Residential access nets
  - insitutional access networks( school, company)
  - mobile access networks (WiFi, 4G/5G)
- What to look for:
- - Transmission rate (bits per second) of access network
  - shared or dedicated access amongst users

## Access Networks: Cable-Based Access
- Cable modem
- Splitter
- Cable headend
- HFC: Hybric fiber coax:
- - Asymmetric up to 40Mbps - 1.2Gbs downstream transmission rate, 30-100 MBps upstream transmission rate
- Network of cable, fiber attaches homes to ISP router
- homes share access network to cable headend

## Access networks: Digital subscriber line (DSL)
- Central office
- Telephone network 
- DSLAM
- voice, data, transmitted at different frequencies over dedicated line to central office
- -use existing telephone line to central office DSLAM
- --data over DSL phone line goes to inteneret
- --voice over DSL phone line goes to telephone net
- 24/52 Mbps dedicated downstream transmission rate
- 3.5-16 Mbps dedicated upstream transmission rate

##Access Networks: Home networks
- Cable or DSL Modem
- Router, firewall, NAT
- Wired Ethernet (1Gbps)
- WiFi wireless access point (54, 450 Mbps)
- Wireless devices often combined in a single box

## Wireless Access networks
- Shared wireless access network connects end system to router via base station
- Wireless local area networks (WLANs): Typically within or around building (~100 ft), 802.11b/g/n (WiFi): 11, 54, 450 Mbps transmission rate
- Wide-area ceullular access networks
- -^ provided by mobile, cellular network operation (10's km)
- -^ 10's Mbps
- -^ 4G Cellular networks (and 5G)

## Access Networks: Enterprise Networks
- Companies, universities, etc
- Mix of wired, wireless link technologies
- Connecting a mix of switches and routers
- Ethernet: wired access at 100 Mbps, 1Gbps, 10Gbps
- WiFi: wireless access points at 11, 54, 450, Mbps

## Packets
Host sending function:
- Takes application message
- breaks into smaller chunks, known as packets of length "L" bits
- transmits packet into access network at transmission rate R
- - Link transmission rate, aka link capacity aka link bandwidth
Transmission delay is L/R -> L(bits)/R(bits/sec) where:
- L is the packet length in bits
- R the link transmission rate in bits per second.


# Links

## Physical Media
- Bit: propagates between transmitter/receiver pairs
- Physical link: What lies between transmitter & reciever
- guided media: signals propagate in solid media, copper, fiber, coax
- unguided media: singals propagate freely. Example: radio waves.

## Twisted Pair (TP)
- Two insulated copper wires
- Category 5: 100Mbps, 1Gbps Ethernet
- Category 6: 10Gbps Ethernet

## Coaxial Cable
- Two concentric copper conductors
- Bidirectional
- Broadband
- - Multiple frequency channels on cable
  - 100's Mbps per channel

## Fibre Optic Cable
- Glass fibre carrying light pulses, each pulse is a bit
- High-speed operation: High-speed point-to-point transmission (10's - 100's Gbps)
- Low error rate: repeaters are spaced far apart and are immune to electromagnetic noise

## Wireless Radio
- Singal carried in electromagnetic spectrum
- No physical "wire"
- Broadcast and half-duplex (sender to receiver)
- Propagation enviroment effects: reflection, obstruction by objects and interference

## Radio Link Types
- Terrestial microwave: up to 45 Mbps channels
- Wirless LAN (WiFi): up to 100 Mbps
- Wide-area (e.g cellular): 4G Cellular: around 10's Mbps
- Satelite: Up to 45 Mbps per channel. 270 milisec end-end delay. geosynchronous versus low-earth-orbit.

# Network Core: Packet/Circuit Switching

## The Network Core
- Mesh of interconnected routers
- Packet-switching: hosts break application layer messages into packets
- forward packets from one router to the next, across links on path from source to destination
- each packet transmitted at full link capacity

## Packet switching: store and forward
- Transmission delay: Takes L/R seconds to transmit L-bit packet into link at R bps
- Store and forward: entire packet must arrive at router before it can be transmitted on next link
- End-end delay: 2L/R (above): assuming zero propagation delay

## Packet switching: Queueing Delay, Loss
- If arrival rate (in bps) to link exceeds transmission rate (bps) of link for a period of time:
> - Packets will queue, waiting to be transmitted on output link
> - Packets can be dropped (lost) if memory buffer in router fills up

## Two key network core functions
- Forwarding:
> - Local action: Moving arriving packets from router's input to appropriate router output link
- Routing:
> - Global action: determine source-destination paths taken by packets
> - routing algorithms

## Alternative to packet switching: circuit switching
- End-end resources allocated to, reserved for "call" between source and destination
- Dedicated resources: no sharing
- Circuit like (gauaranteed) performance
- Circuit segment idle (if not used by call, no sharing)
- Communly used in traditional telephone networks

## Circuit switching: FDM and TDM
- Frequency Division Muliplexing (FDM):
> - Optical electromagnetic frequencies divided into narrow frequency bands
> - Each call is allocated its own band, can transmit at max rate of that narrow band
- Time Division Mulitplexing (TDM):
> - Time divided into slots
> - each call allocated peridodic slot(s), can transmit at maximum rate of (wider) frequency band, but only during its time slot(s)

## Packet Switching vs Circuit Switching
Example: 1Gb/s link
Each user:
> 100 Nb/s when "active"
> active 10% of the time
- Circuit switching: 10 users
- Packet switching: With 35 users, probability > 10 active at same time is less than 0.0004*
- Packet switching allows more users to use the network

## Packet switching: Satistical Multiplexing
How did we get the above 0.0004?
![image](https://github.com/user-attachments/assets/5ce1fbcf-4f22-42b1-9374-4fa72ad810c4)

## Packet vs Circuit
- Packet  switching is great for burst data as its simpler

## Internet structure: A network of networks
- Hosts connect to internet via access internet service providers (ISPs)
> - Residential, enterprise (company, university, commercial) ISPs
- Access ISPs in turn must be interconnected
> - So that any two hosts can send packets to each other
- Resulting network of networks is very complex
> - Evolution was driven by economics and national policies

## Scaling challenges
- Connecting each access ISP to eahc other directly doesn't scale O(N^2) connections
- With N ISPs, each ISP needs to connect to (N-1) other ISPs
- Total Connections = (N*(N-1))/2
> - Each ISP must establish a direct link with every other ISP
> - As N (the number of ISPs) increases, the required connections grows quadratically

## How is this solved in practice?
The internet uses a hierarhical structure to address teh scalability issue:
- Core ISPs (Tier 1 ISPs):
> - A few, high-capacity large scale ISPs from the backbone of the internet and interconnect directly
- Peering agreeements
> - ISPs use peering at internet exchange points (IXPs) to share traffic with multiple ISPs simultaneously, reducing the need for direct links
- Transit ISPs:
> - Smaller ISPs user larger ISPs to route traffic indirectly to other ISPs

## Internet structure: Multi-Tiered Hierachy
- Tier 1 ISPs: Large, national/international coverage
- Regional ISPs: Connect access networks to ISPs
- Content provider networks: (e.g google) run their own network to bring services close to end users.

## Internet structure: a network of networks
- A packet passes through many networks
- Tier 3 ISPs and local ISPs are customers of higher tier ISPs connecting them to the rest of the internet
- Tier 2 ISPs: smaller (often regional) ISPs. Connect to one or more tier 1 ISPs possibly other tier 2 ISPs

# Challenges in Networked Systems

## Asynchronous Operation
- Fundamental constraint: speed of light
- Consider:
> - How many cycles does your 3GHZ CPU in London execute before it can possibly get a response from a message it sends to a server in Palo Alto?
> - London to Palo Alto: 8,609 km
> - Travelling at 300,000 km/s: 28.7 miliseconds
> - Then back to london: 2*28.7 = 57.39 miliseconds
> - 3,000,000,000 cycles/sec * 0.05739 = 172,179,999 cyles
- Communication feedback is always dated

## Prone to failure
- To send a message, all components along a path must function correctly
> - Software, wireless acces point, firewall, links, network interface cards, switches..
> - including human operators
- Consider: 50 components in a system, each working correctly 99% of the time -> 39.5% chance communication will fail
- Plus, recall:
> - Scale -> Lots of components
> - Asynchrony -> Takes a long time to hear (bad) news
> - Federation (internet) -> hard to indetify fault or assign blame

## Cumulative Reliability
- System Overview:
> - The system has 50 components
> - Eahc component works correctly 99% of the time (or has a failure rate of 1-0.99 = 0.01)
- System Reliability:
> - For the system to work, all 50 components must function correctly at the same time.
> - The probability of a single component working correctly is P(correct) = 0.99
> - Assuming the components are independent, the probabiltiy all 50 components is
> - ![image](https://github.com/user-attachments/assets/fda15ad3-c908-47a8-bcdb-6536fca77d18)
- Chance of Failure:
> - The probability of the system failing is:
> - ![image](https://github.com/user-attachments/assets/e400c3e0-0522-44f5-9075-069ac7cd65cc)

## Interpretation
- Even though each individual component is highly reliable (99%), the reliability of the entire system drops significantly because the failure of any single component leads to a system failure. 
