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
