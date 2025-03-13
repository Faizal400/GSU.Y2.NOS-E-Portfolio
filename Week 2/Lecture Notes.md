# 🖥️ Week 2 - Application Layer

*The TCP/IP Model consists of 4 Layers. This week covers the first layer - The Application Layer*

**Why Layering?**
- Allows for different software packages (applications) to use the same transport, network and link layers but have their own application layer
- The program encodes the message, the rest of the communcation method remains the same.

## Application Layer Overview

> - Principles of network applications
> - Web and HTTP
> - Email, SMTP, IMAP
> - Domain Name System (DNS)
> - P2P Applications
> - Video Streaming and Content Distribution Networks (CDNs)
> - Socket Programming with UDP and TCP

### Network Application Examples
> - Social networking
> - Web browsing
> - Text messaging
> - Email
> - Multi-user network games
> - Streaming video (YouTube, Netflix)
> - P2P file sharing
> - Voice over IP (e.g., Skype)
> - Real-time video conferencing
> - Internet search
> - Remote login
*These applications run across various networks, including mobile, home, enterprise and the broader internet.*

## Creating a Network App
- Involves writing programs that:
  - Can run on (different) end systems
  - Communcate over a network
  - Example: Web server software communcates with browser software
- No need to write software for network-core devices
- Applicatoins on end systems allow for rapid application development and propagataion

### Client-Server Paradigm
- Server:
  - Always-on host
  - Permanent IP address
  - Often in data centers for scaling
- Clients:
  - Contact and communicate with the server
  - May be intermittently connected
  - May have dynamic IP address
  - Do not communicate directly with each other
  - Examples: HTTP, IMAP, FTP

### P2P Architecture
- No always-on server.
- Arbitrary end systems communicate directly.
- Peers request and provide services to each other.
- Self-scalability: New peers bring new service capacity as well as demands.
- Peers are intermittently connected and change IP addresses, leading to complex management.
- Example: P2P file sharing.

### Process Communicating
- Process: Program running within a host
  - Within the same host, processes communicate user inter-process communcation (which is defined by the OS)
  - Processes in different hosts communicate by exchanging messages
- Client ProcessL Initiates communications
- Server Process: Waits to be contacted.
- Applications with P2P architectures have both client and server processes.

### Sockets
- Process sends/receives messages to/from its socket.
- Socket is analogous to a door.
  - Sending process shoves messages out the door
  - Sending process relies on transport infastructure to deliver the message to the socket at the receiving process
- Two sockets involved: one on each side

### Addressing Processes
- To receive messages, a process must have an identifier.
- A host device has a unique 32-bit IP address.
- The identifier includes both the IP address and port numbers associated with the process on the host.
- Example port numbers:
  - HTTP Server: 90
  - Mail Server: 25
- To send a HTTP message to a web server:
  - IP address: 128.119.245.12
  - Port number: 80

### Application-Layer Protocol Definition
An application-layer protocol defines:
- Types of messages exchanged (e.g., request, response).
- Message syntax (fields in messages and how they are delineated).
- Message semantics (meaning of information in fields).
- Rules for when and how processes send and respond to messages.

### Protocol Types
- Open Protocols:
  - Defined in RFCs (Request for Comments).
  - Everyone has access to the protocol definition.
  - Allows for interoperability.
  - Examples: HTTP, SMTP.
- Proprietary Protocols:
  - Example: Skype.

## Transport Service Requirements
What transport service does an app need?

### Data Integrity
- Some apps (e.g., file transfer, web transactions) require 100% reliable data transfer.
= Other apps (e.g., audio) can tolerate some loss.

### Timing
- Some apps (e.g., Internet telephony, interactive games) require low delay to be "effective".

### Throughout
- Some apps (e.g., multimedia) require a minimum amount of throughput to be "effective".
- Other apps ("elastic apps") make use of whatever throughput they get.

### Security
- Encryption, data integrity.

### Transport Service Requirements: Common Apps
![image](https://github.com/user-attachments/assets/bb19ddfe-019c-4d84-80e8-a1606350c0d2)



### Internet Transport Protocols Services
- TCP Service:
  - Reliable transport between sending and receiving process.
  - Flow control: sender won’t overwhelm receiver.
  - Congestion control: throttle sender when network overloaded.
  - Does not provide: timing, minimum throughput guarantee, security.
  - Connection-oriented: setup required between client and server processes.
- UDP Service:
  - Unreliable data transfer between sending and receiving process.
  - Does not provide: reliability, flow control, congestion control, timing, throughput guarantee, security, or connection setup.

### Why UDP?
- No connection establishment (which can add delay).
- Simple: no connection state at sender, receiver.
- Small header size.
- No congestion control: can blast away as fast as desired.

### Internet Transport Protocols
![image](https://github.com/user-attachments/assets/878a1f5c-1394-4b98-9905-871212f2044a)

## Web and HTTP
### Web Page Components
- A web page consists of objects, each of which can be stored on different web servers.
- An object can be an HTML file, JPEG image, Java applet, or audio file.
- A web page consists of a base HTML file which includes several referenced objects, each addressable by a URL (Uniform Resource Locator).
  - Example: www.someschool.edu/someDept/pic.gif
    - Host name: www.someschool.edu
    - Path name: /someDept/pic.gif

### HTTP Overview
- **HTTP (Hypertext Transfer Protocol):**
  - Web's application layer protocol.
  - Client/server model:
    - Client: browser that requests, receives, and displays web objects.
    - Server: web server sends objects in response to requests.
- HTTP uses TCP:
  - Client initiates TCP connection (creates socket) to server, port 80.
  - Server accepts TCP connection from client.
  - HTTP messages (application-layer protocol messages) exchanged between browser (HTTP client) and web server (HTTP server).
  - TCP connection closed.

### HTTP is Stateless
- Server maintains no information about past client requests.
- Protocols that maintain "state" are complex.
- Past history (state) must be maintained.
- If server/client crashes, their views of "state" may be inconsistent and must be reconciled.

### HTTP Connections: Two Types
- **Non-Persistent HTTP:**
  - TCP connection opened.
  - At most one object sent over TCP connection.
  - TCP connection closed.
- Downloading multiple objects requires multiple connections.
- **Persistent HTTP:**
  - TCP connection opened to a server.
  - Multiple objects can be sent over a single TCP connection between the client and that server.
  - TCP connection closed.

### Non-Persistent HTTP Example
- User enters URL: **www.someSchool.edu/someDepartment/home.index**
  - HTTP client initiates TCP connection to HTTP server (process) at www.someSchool.edu on port 80.
  - HTTP server at host www.someSchool.edu waiting for TCP connection at port 80 "accepts" connection, notifying client.
- HTTP client sends HTTP request message (containing URL) into TCP connection socket. The message indicates that client wants the object someDepartment/home.index.
- HTTP server receives the request message, forms a response message containing the requested object (containing text, references to 10 JPEG images), and sends the message into its socket.
- HTTP server closes the TCP connection.
- HTTP client receives the response message containing the HTML file, displays HTML. Parsing the HTML file, finds 10 referenced JPEG objects.
- Steps 1-5 are repeated for each of the 10 JPEG objects.

### Non-Persistent HTTP: Response Time
- **RTT (Round-Trip Time):** Time for a small packet to travel from client to server and back.
- **HTTP Response Time (per object):**
  - One RTT to initiate TCP connection.
  - One RTT for HTTP request and first few bytes of HTTP response to return.
  - Object/file transmission time.
- Non-persistent HTTP response time = 2RTT + file transmission time.

### Non-Persistent HTTP Issues
- Requires 2 RTTs per object.
- OS overhead for each TCP connection.
- Browsers often open multiple parallel TCP connections to fetch referenced objects in parallel.

### Persistent HTTP (HTTP 1.1)
- Connection stays open after the server responds to the first request.
- The same connection can be used to send and receive more requests and responses between the client and server.
- The client can quickly send new requests for other objects (like images or scripts) without waiting to start a new connection.
- Saves time by avoiding multiple connection setups.
- Reduced delay: Loading a web page with many parts (like images or stylesheets) is much faster because the client and server reuse the same connection.

### Types of HTTP Messages
- Two types: Request and Response.

### HTTP Request Message
- ASCII (human-readable format).
- Request line (GET, POST, HEAD commands).
- Header lines.
- Carriage return, line feed at the start of the line indicates the end of header lines.
![image](https://github.com/user-attachments/assets/99986c39-6ecd-417f-aa2a-b6a8785ec017)


### HTTP Request Message: General Format
![image](https://github.com/user-attachments/assets/411b61ba-d669-4d08-896b-956411444b4a)

Where:

- **method:** e.g. GET, POST, HEAD. Specifies the action the client wants the server to perform.
- **sp:** space.
- **cr:** carriage return character.
- **lf:** line feed character.

### Other HTTP Request Methods
- **POST Method:**
  - Sends data (like form information) from your browser to the server.
  - The data is sent in the body of the request, not visible in the URL.
  - Example use: Submitting a registration form or uploading a file.
- **GET Method:**
  - Retrieves data from a server.
  - Any information you send (like search terms) is added to the URL after a ?.
  - Example use: Searching for something on Google.
  - www.somesite.com/animalsearch?monkeys&banana
- **HEAD Method:**
  - Asks only for the headers (metadata) of a page, not the full content.
  - Useful for checking if a resource exists or seeing its size without downloading it.
  - Example use: Checking if a file is available on a server.
**PUT Method:**
- Uploads a file or resource to a server.
- If the file already exists at that URL, it gets replaced with the new one.
- Example use: Updating a document or image on a server.

### HTTP Response Message
![image](https://github.com/user-attachments/assets/e0582f77-a3e7-4696-a4e7-ccb655472464)

### HTTP Response Status Codes
- Status code appears in the first line in the server-to-client response message.
- Some sample codes:
  - 200 OK: Request succeeded; the requested object is later in this message.
  - 301 Moved Permanently: Requested object moved; the new location is specified later in this message (in the Location: field).
  - 400 Bad Request: Request message not understood by the server.
  - 404 Not Found: The requested document was not found on this server.
  - 505 HTTP Version Not Supported: The HTTP version used in the request is not supported by the server.

### Maintaining User/Server State: Cookies
- HTTP GET/response interaction is stateless.
- No notion of multi-step exchanges of HTTP messages to complete a Web "transaction".
- No need for the client/server to track the "state" of a multi-step exchange.
- All HTTP requests are independent of each other.
- No need for the client/server to "recover" from a partially-completed-but-never-completely-completed transaction.

### How Cookies Work
Websites and client browsers use cookies to maintain some state between transactions. Four components:
- Cookie header line of HTTP response message
- Cookie header line in next HTTP request message
- Cookie file kept on user’s host, managed by user’s browser
- Back-end database at Website

### Example:
- Susan uses a browser on a laptop, visits a specific e-commerce site for the first time.
- When the initial HTTP requests arrive at the site, the site creates:
  - A unique ID (aka "cookie").
  - An entry in the backend database for the ID.
- Subsequent HTTP requests from Susan to this site will contain the cookie ID value, allowing the site to "identify" Susan.

### Web Caches (Proxy Servers)
- The user configures the browser to point to a web cache.
- The browser sends all HTTP requests to the cache.
  - If the object is in the cache, the cache returns the object to the client.
  - Else, the cache requests the object from the origin server, caches the received object, then returns the object to the client.
- Goal: satisfy client request without involving origin server.
- Web cache acts as both client and server:
  - Server for the original requesting client.
  - Client to the origin server.
- Typically, the cache is installed by an ISP (university, company, residential ISP).

### Why Web Caching?
- Reduce response time for the client request.
- Reduce traffic on an institution’s access link.
- The Internet is dense with caches.
- Enables "poor" content providers to more effectively deliver content.

### Caching Example
- Scenario:
  - Access link rate: 1.54 Mbps
  - RTT from institutional router to server: 2 sec
  - Web object size: 100K bits
  - Average request rate from browsers to origin servers: 15/sec
- Average data rate to browsers: 1.50 Mbps
- Performance:
  - LAN utilization: .0015
  - Access link utilization = .97
  - End-to-end delay = Internet delay + access link delay + LAN delay = 2 sec + minutes + usecs

### Caching Example: Buy a Faster Access Link
- Scenario:
  - Access link rate: 154 Mbps
  - RTT from institutional router to server: 2 sec
  - Web object size: 100K bits
  - Average request rate from browsers to origin servers: 15/sec
  - Average data rate to browsers: 1.50 Mbps
- Performance:
  - LAN utilization: .0015
  - Access link utilization = . 0097
  - End-to-end delay = Internet delay + msecs + usecs
- Cost: Faster access link (expensive!).

### Caching Example: Install a Web Cache
- Scenario:
  - Access link rate: 1.54 Mbps
  - RTT from institutional router to server: 2 sec
  - Web object size: 100K bits
  - Average request rate from browsers to origin servers: 15/sec
  - Average data rate to browsers: 1.50 Mbps
- Cost: Web cache (cheap!).
- Calculating access link utilization, end-to-end delay with cache:
  - Suppose the cache hit rate is 0.4: 40% of requests are satisfied at the cache, 60% of requests are satisfied at the origin.
  - Access link: 60% of requests use the access link.
  - Data rate to browsers over access link = 0.6 * 1.50 Mbps = .9 Mbps
  - Utilization = 0.9/1.54 = .58
  - Average end-to-end delay = 0.6 * (delay from origin servers) + 0.4 * (delay when satisfied at cache) = 0.6 (2.01) + 0.4 (~msecs) = ~ 1.2 secs
  - Lower average end-to-end delay than with 154 Mbps link (and cheaper too!).

### Conditional GET
- Goal: don’t send object if the cache has an up-to-date cached version.
- No object transmission delay.
- Lower link utilization.
- Cache: specify the date of cached copy in HTTP request: If-modified-since: <date>
- Server: the response contains no object if the cached copy is up-to-date: HTTP/1.0 304 Not Modified
![image](https://github.com/user-attachments/assets/ce18d924-9a66-41c3-ad70-74e7a94d6e84)
![image](https://github.com/user-attachments/assets/e5aeedfa-64c0-429d-87f9-fb9c624b6479)

### HTTP/2
- Key Goal: Decreased delay in multi-object HTTP requests.
- HTTP1.1: introduced multiple, pipelined GETs over a single TCP connection.
  - The server responds in-order (FCFS: first-come-first-served scheduling) to GET requests.
  - With FCFS, a small object may have to wait for transmission (head-of-line (HOL) blocking) behind large object(s).
  - Loss recovery (retransmitting lost TCP segments) stalls object transmission.
- HTTP/2: [RFC 7540, 2015] increased flexibility at server in sending objects to the client:
  - Methods, status codes, and most header fields are unchanged from HTTP 1.1.
  - The transmission order of requested objects is based on client-specified object priority (not necessarily FCFS).
  - Push unrequested objects to the client.
  - Divide objects into frames, schedule frames to mitigate HOL blocking.

### HTTP/2: Mitigating HOL Blocking
- HTTP 1.1: The client requests 1 large object (e.g., a video file) and 3 smaller objects.
- Objects are delivered in the order requested: O2, O3, O4 wait behind O1.
- HTTP/2: Objects are divided into frames, and frame transmission is interleaved.
- O2, O3, and O4 are delivered quickly; O1 is slightly delayed.

### HTTP/2 to HTTP/3
- Key Goal: Decreased delay in multi-object HTTP requests.
- HTTP/2 over a single TCP connection means:
  - Recovery from packet loss still stalls all object transmissions.
  - As in HTTP 1.1, browsers have an incentive to open multiple parallel TCP connections to reduce stalling and increase overall throughput.
  - No security over vanilla TCP connection.
- HTTP/3: adds security, per-object error, and congestion control (more pipelining) over UDP.

## E-mail
### Three Major Components
- User agents.
- Mail servers.
- Simple Mail Transfer Protocol (SMTP).
> **__User Agent___**
- Also known as a “mail reader”.
- Composing, editing, and reading mail messages.
- Examples: Outlook, iPhone mail client.
- Outgoing and incoming messages are stored on the server.
> **__Mail Servers__**
- Mailbox contains incoming messages for the user.
- Message queue of outgoing (to be sent) mail messages.
- SMTP protocol between mail servers to send email messages.
  - Client: sending mail server.
  - "Server": receiving mail server.

### E-mail: the RFC (5321)
- Uses TCP to reliably transfer email messages from the client (mail server initiating the connection) to the server, port 25.
- Direct transfer: sending server (acting like client) to receiving server.
- Three phases of transfer:
  - Handshaking (greeting).
  - Transfer of messages.
  - Closure.
- Command/response interaction (like HTTP).
  - Commands: ASCII text.
  - Response: status code and phrase.
- Messages must be in 7-bit ASCII.

### Scenario: Alice Sends Email to Bob
- Alice uses UA to compose an email message “to” bob@someschool.edu.
- Alice’s UA sends the message to her mail server; the message is placed in the message queue.
- The client side of SMTP opens a TCP connection with Bob’s mail server.
- The SMTP client sends Alice’s message over the TCP connection.
- Bob’s mail server places the message in Bob’s mailbox.
- Bob invokes his user agent to read the message.
- Sample SMTP Interaction
![image](https://github.com/user-attachments/assets/00077143-7f2f-4b36-88b3-be00a4e1a6ca)


### SMTP: Closing Observations
- SMTP uses persistent connections.
- SMTP requires the message (header & body) to be in 7-bit ASCII.
- SMTP server uses CRLF.CRLF to determine the end of the message.
- Comparison with HTTP:
  - HTTP: pull.
  - SMTP: push.
  - Both have ASCII command/response interaction and status codes.
  - HTTP: each object is encapsulated in its own response message.
  - SMTP: multiple objects are sent in a multipart message.
### Mail Message Format
- SMTP: protocol for exchanging email messages, defined in RFC 531 (like HTTP).
- RFC 822 defines the syntax for the email message itself (like HTML).
  - Header lines, e.g.:
    - To:
    - From:
    - Subject:
    - These lines within the body of the email message area are different from SMTP MAIL FROM:, RCPT TO: commands!
  - Body: the “message”, ASCII characters only.

### Mail Access Protocols
- SMTP: delivery/storage of email messages to the receiver’s server.
- Mail access protocol: retrieval from the server.
  - IMAP (Internet Mail Access Protocol) [RFC 3501]: messages stored on the server, IMAP provides retrieval, deletion, and folders of stored messages on the server.
  - HTTP: Gmail, Hotmail, Yahoo! Mail, etc., provide a web-based interface on top of STMP (to send), IMAP (or POP) to retrieve email messages.

## Domain Name System (DNS)
### People vs Internet Hosts
- People: many identifiers (SSN, name, passport #).
- Internet hosts and routers:
  - IP address (32 bit) - used for addressing datagrams.
  - “Name”, e.g., cs.umass.edu - used by humans.
- Question: how to map between IP address and name, and vice versa?

### Domain Name System
- Distributed database implemented in a hierarchy of many name servers.
- Application-layer protocol: hosts, name servers communicate to resolve names (address/name translation).
  - Core Internet function, implemented as application-layer protocol.
  - Complexity at the network’s “edge”.

### DNS: Services, Structure
- Question: Why not centralize DNS?
  - Single point of failure.
  - Traffic volume.
  - Distant centralized database.
  - Maintenance.
Answer: Doesn’t scale!
- Comcast DNS servers alone: 600B DNS queries per day.

### DNS Services
- Hostname to IP address translation.
- Host aliasing.
  - Canonical, alias names.
- Mail server aliasing.
- Load distribution.
  - Replicated Web servers: many IP addresses correspond to one name.

### DNS: A Distributed, Hierarchical Database
- Root DNS Servers:
  - Official, contact-of-last-resort by name servers that cannot resolve a name.
  - Incredibly important Internet function.
    - The Internet couldn’t function without it!
    - DNSSEC – provides security (authentication and message integrity).
  - ICANN (Internet Corporation for Assigned Names and Numbers) manages the root DNS domain.
    - 13 logical root name “servers” worldwide.
    - Each “server” is replicated many times (~200 servers in the US).
- Top-Level Domain (TLD) Servers:
  - Responsible for .com, .org, .net, .edu, .aero, .jobs, .museums, and all top-level country domains, e.g., .cn, .uk, .fr, .ca, .jp.
  - Network Solutions: authoritative registry for .com, .net TLD.
  - Educause: .edu TLD.
- Authoritative DNS Servers:
  - Organization’s own DNS server(s), providing authoritative hostname to IP mappings for the organization’s named hosts.
  - Can be maintained by the organization or a service provider.
- Local DNS Name Servers:
  - Does not strictly belong to the hierarchy.
  - Each ISP (residential ISP, company, university) has one.
    - Also called the “default name server”.
  - When a host makes a DNS query, the query is sent to its local DNS server.
    - Has a local cache of recent name-to-address translation pairs (but may be out of date!).
    - Acts as a proxy, forwards the query into the hierarchy.

### DNS Name Resolution: Iterated Query
- Example: A host at engineering.nyu.edu wants the IP address for gaia.cs.umass.edu.
- Iterated query:
  - The contacted server replies with the name of the server to contact.
  - “I don’t know this name, but ask this server”.

### DNS Name Resolution: Recursive Query
- Recursive query:
  - Puts the burden of name resolution on the contacted name server.
  - Heavy load at upper levels of the hierarchy.

### Caching, Updating DNS Records
- Once (any) name server learns a mapping, it caches the mapping.
  - Cache entries timeout (disappear) after some time (TTL).
  - TLD servers are typically cached in local name servers.
  - Thus, root name servers are not often visited.
- Cached entries may be out-of-date (best-effort name-to-address translation!).
  - If a name host changes its IP address, it may not be known Internet-wide until all TTLs expire!
- Update/notify mechanisms proposed IETF standard.

### DNS Records
- DNS: distributed database storing resource records (RR).
- RR format: (name, value, type, ttl)
  - type=A:
    - Name is hostname.
    - The value is the IP address.
  - type=NS:
    - Name is domain (e.g., foo.com).
    - The value is the hostname of the authoritative name server for this domain.
  - type=CNAME:
    - Name is an alias name for some “canonical” (the real) name.
    - www.ibm.com is really servereast.backup2.ibm.com.
    - The value is the canonical name.
  - type=MX:
    - The value is the name of the mailserver associated with the name.

### DNS Protocol Messages
- DNS query and reply messages, both have the same format:
    - Message header:
      - Identification: 16-bit # for the query; the reply to the query uses the same #.
      - Flags:
        - Query or reply.
        - Recursion desired.
        - Recursion available.
        - Reply is authoritative.
- Question section: contains information about the query being made, such as the name being looked up and the type of record being requested.
- Answer section: contains the resource records (RRs) that answer the query. These RRs provide the information that the client was looking for, such as the IP address for a given hostname.
- Authority section: contains RRs that point to authoritative name servers for the queried domain. These RRs help in the process of name resolution by guiding the resolver to the servers that have the most accurate information.
- Additional section: contains additional "helpful" information that may be used in the name resolution process. For example, it might include IP addresses for the name servers listed in the authority section.

### Inserting Records into DNS
- Example: A new startup, “Network Utopia”.
  - Register the name networkuptopia.com at a DNS registrar (e.g., Network Solutions).
    - Provide names and IP addresses of the authoritative name server (primary and secondary).
  - The registrar inserts NS and A RRs into the .com TLD server:
    - (networkutopia.com, dns1.networkutopia.com, NS)
    - (dns1.networkutopia.com, 212.212.212.1, A)
  - Create an authoritative server locally with IP address 212.212.212.1.
    - Type A record for www.networkuptopia.com.
    - Type MX record for networkutopia.com.

### DNS Security
- DDoS Attacks:
  - Bombard root servers with traffic.
  - Not successful to date.
  - Traffic filtering.
  - Local DNS servers cache IPs of TLD servers, allowing root server bypass.
  - Bombard TLD servers.
  - Potentially more dangerous.
- Redirect Attacks:
  - Man-in-the-middle.
  - Intercept DNS queries.
  - DNS poisoning.
  - Send bogus replies to DNS server, which caches them.
- Exploit DNS for DDoS:
-  Send queries with a spoofed source address: target IP.
-   Requires amplification.
- DNSSEC [RFC 4033]:
  - Provides authentication and message integrity.

### Peer-to-Peer (P2P) Architecture
- No always-on server.
- Arbitrary end systems communicate directly.
- Peers request service from other peers and provide service in return to other peers.
- Self-scalability: new peers bring new service capacity and new service demands.
- Peers are intermittently connected and change IP addresses.
- Complex management.
- Examples: P2P file sharing (BitTorrent), streaming (KanKan), VoIP (Skype).

### File Distribution: Client-Server vs P2P
Question: how much time to distribute a file (size F) from one server to N peers?
Peer upload/download capacity is a limited resource.
- us: server upload capacity.
- ui: peer i upload capacity.
- di: peer i download capacity.
![image](https://github.com/user-attachments/assets/3531d327-0da2-451e-8e6a-0292b1644912)

### File Distribution time: Client - server
- **Server Transmission**: Must sequentially send (upload) N file copies:
  - time to send one copy: F/us
  - time to send N copies: NF/us
- **Client**: Each client must download file copy
  - dmin = min client download rate
  - min client download time: F/dmin
![image](https://github.com/user-attachments/assets/677da478-13c9-4a8b-b9df-d217271e17e9)

### File Distribution: P2P
- **Server Transmission**: Must upload atleast one copy:
  - Time to send one copy: F/us
- **Client**: each client must download file copy
  - min client download time: F/dmin
- **Clients:** as aggregate must download NF bits
  - max upload rate (limiting download rate) is: ![image](https://github.com/user-attachments/assets/e21b4530-3cf1-4058-a60b-071ac46dffb6)
![image](https://github.com/user-attachments/assets/741099b8-73df-4b0b-bf1f-c9fdc3dc78ba)


### **Client-Server Model** 
- A **centralized server** manages and provides resources to clients.  
- **Example:** Web browsing (HTTP), Email (SMTP, IMAP), DNS.  
- **Advantage:** Centralized control, efficient management.  
- **Disadvantage:** Can create **bottlenecks**, higher costs.  

### **Peer-to-Peer (P2P) Model**   
- Peers (users) act as **both clients and servers**, sharing resources directly.  
- **Example:** BitTorrent, Skype, decentralized file-sharing.  
- **Advantage:** Scalable, more resilient.  
- **Disadvantage:** Harder to manage, potential security risks.  

**Key Takeaway:** P2P networks are **more scalable**, while client-server models are **easier to control**.

---

### P2P File Distribution: BitTorrent  
BitTorrent **improves file sharing** by **breaking files into chunks** and distributing them among peers.  
- **Tracker:** A server that keeps track of participating peers.  
- **Torrent:** A group of peers exchanging file chunks.  
- **Tit-for-Tat Strategy:** Peers prioritize uploading to others who also upload in return.  

 **Why is BitTorrent Efficient?**  
- Uses **distributed resources**, reducing reliance on a **centralized server**.  
- **Peers contribute bandwidth**, making file distribution **faster**.  

---

## Video Streaming & Content Distribution Networks (CDNs)  
### **Why is Streaming Challenging?**   
- **High bandwidth usage** (Netflix, YouTube consume **80% of ISP traffic**).  
- **Heterogeneous users** (wired, mobile, fast/slow connections).  

### **How CDNs Solve This**  
CDNs **store multiple copies of content** in different geographical locations to reduce latency.  
- **Example:** Netflix uses **CDN nodes** to deliver video from the nearest server.  

### **DASH (Dynamic Adaptive Streaming over HTTP)**  
- **Divides video into chunks** stored at different quality levels.  
- The **client selects the best chunk** based on available bandwidth.  
- Prevents buffering issues, improving video playback quality.  

**Key Takeaway:** CDNs optimize video streaming by **reducing congestion** and improving **load times**.

---

## 5. Socket Programming: Enabling Network Communication  
Sockets allow **applications to send and receive data** over a network.  

### **UDP (User Datagram Protocol)**  
- **No connection setup**, sends data as independent packets.  
- **Unreliable**, but fast (used for gaming, live streaming).  
- **Example:** Sending a message with UDP:  
  ```python
  from socket import *
  clientSocket = socket(AF_INET, SOCK_DGRAM)
  clientSocket.sendto(b"Hello, Server", ("127.0.0.1", 12000))
  ```
  
