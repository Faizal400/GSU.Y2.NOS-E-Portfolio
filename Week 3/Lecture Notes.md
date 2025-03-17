# Transport Layer

## Transport Layer Overview
### Goal
- Understand the principles behind transport layer services:
  - Multiplexing and demultiplexing
  - Reliable data transfer
  - Flow control
  - Congestion control
- Learn about Internet transport layer protocols:
  - UDP: Connectionless transport
  - TCP: Connection-oriented reliable transport
  - TCP congestion control
### Roadmap
- Transport-layer services
- Multiplexing and demultiplexing
- Connectionless transport: UDP
- Principles of reliable data transfer
- Connection-oriented transport: TCP
- Principles of congestion control
- TCP congestion control
- Evolution of transport-layer functionality

## Transport Services and Protocols
### Logical Communication
- Provides logical communication between application processes running on different hosts.
- Example network topology: Mobile network, home network, enterprise network, ISP networks, datacenter networks, content provider networks.

### Transport Protocols Actions in End Systems
- Sender:
  - Breaks application messages into segments.
  - Passes segments to the network layer.
- Receiver:
  - Reassembles segments into messages.
  - Passes messages to the application layer.

### Available Transport Protocols
- Two transport protocols available to Internet applications:
  - TCP (Transmission Control Protocol)
  - UDP (User Datagram Protocol)

### Sender Actions
- Receives an application-layer message.
- Determines segment header field values.
- Creates a segment.
- Passes the segment to IP (Internet Protocol).

### Receiver Actions
- Receives a segment from IP.
- Extracts the application-layer message.
- Checks header values.
- Demultiplexes the message up to the application via a socket.

## Two Principal Internet Transport Protocols
### TCP (Transmission Control Protocol)
- Reliable, in-order delivery.
- Congestion control.
- Flow control.
- Connection setup.

### UDP (User Datagram Protocol)
- Unreliable, unordered delivery.
- No-frills extension of "best-effort" IP.

### Services Not Available
- Delay guarantees.
- Bandwidth guarantees.

## Multiplexing and Demultiplexing
### Multiplexing
- At the sender: Handle data from multiple sockets and add a transport header (later used for demultiplexing).
### Demultiplexing
- At the receiver: Use header information to deliver received segments to the correct socket.
### How Demultiplexing Works
- The host receives IP datagrams.
  - Each datagram has a source IP address and a destination IP address.
  - Each datagram carries one transport-layer segment.
  - Each segment has a source port number and a destination port number.
- The host uses IP addresses and port numbers to direct the segment to the appropriate socket.
**TCP/UDP Segment Format**
__The format for TCP/UDP segment is:__
> [insert table image]

### Connectionless Demultiplexing
Recall:
- When creating a socket, a host-local port number must be specified.
> - DatagramSocket mySocket1 = new DatagramSocket (12534);
- When a receiving host receives a UDP segment:
  - It checks the destination port number in the segment.
  - It directs the UDP segment to the socket with that port number.
- When creating a datagram to send into a UDP socket, you must specify:
  - Destination IP address.
  - Destination port number.
- IP/UDP datagrams with the same destination port number but different source IP addresses and/or source port numbers will be directed to the same socket at the receiving host.

-# Connectionless Demultiplexing Example
```python
DatagramSocket serverSocket = new DatagramSocket(6428);
DatagramSocket mySocket1 = new DatagramSocket(5775);
DatagramSocket mySocket2 = new DatagramSocket(9157);
```
- P1 sends to serverSocket (dest port 6428) from port 9157
- P3 sends to mySocket1 (dest port 5775) from unknown port
- P4 sends to mySocket2 (dest port 9157) from unknown port

### Connection-Oriented Demultiplexing
- TCP socket is identified by a 4-tuple:
  - Source IP address
  - Source port number
  - Destination IP address
  - Destination port number
- A server may support many simultaneous TCP sockets:
  - Each socket is identified by its own 4-tuple.
  - Each socket is associated with a different connecting client.
- Demux: The receiver uses all four values (4-tuple) to direct the segment to the appropriate socket.
Connection-Oriented Demultiplexing Example
- Host A (IP: A) port 9157 sends to Server B (IP: B) port 80
- Host C (IP: C) port 5775 sends to Server B (IP: B) port 80
- Host C (IP: C) port 9157 sends to Server B (IP: B) port 80
- Three segments destined to IP address B, destination port 80, are demultiplexed to different sockets based on the source IP and port.
### Summary of Multiplexing/Demultiplexing
- Multiplexing/demultiplexing is based on segment/datagram header field values.
- **UDP:** Demultiplexing using the destination port number only.
- **TCP:** Demultiplexing using the 4-tuple: source and destination IP addresses and port numbers.
- Multiplexing/demultiplexing happen at all layers.

## Connectionless Transport: UDP
### UDP: User Datagram Protocol
- "No frills," "bare bones" Internet transport protocol.
- "Best effort" service; UDP segments may be:
  - Lost
  - Delivered out-of-order to the application.
- No connection establishment (which can add RTT delay).
- Simple: No connection state at sender or receiver.
- Small header size.
- No congestion control.
- UDP can blast away as fast as desired.
- Can function in the face of congestion.

### Why is there a UDP?
- Connectionless:
  - No handshaking between UDP sender and receiver.
  - Each UDP segment is handled independently of others.
### UDP Use Cases
- Streaming multimedia applications (loss-tolerant, rate-sensitive).
- DNS (Domain Name System)
- SNMP (Simple Network Management Protocol)
- HTTP/3

### Reliable Transfer over UDP
- If reliable transfer is needed over UDP (e.g., HTTP/3):
  - Add needed reliability at the application layer.
  - Add congestion control at the application layer.

### UDP Segment Header Format
[insert table png]
- Length: Length, in bytes, of the UDP segment, including the header.

### UDP Checksum
Goal
- Detect errors (i.e., flipped bits) in the transmitted segment.
Process
- Sender:
  - Treat the contents of the UDP segment (including UDP header fields and IP addresses) as a sequence of 16-bit integers.
  - Checksum: Addition (one’s complement sum) of the segment content.
  - The checksum value is put into the UDP checksum field.
- Receiver:
  - Compute the checksum of the received segment.
  - Check if the computed checksum equals the checksum field value:
    - Not equal: Error detected.
    - Equal: No error detected (but errors may still exist).
Example
Transmitted Data (16-bit integers):
```
5 (0000000000000101)
6 (0000000000000110)
11 (0000000000001011)
```
Sum the integers: 
`5+6+11=22`
`5+6+11=22 (0000000000010110)`
Checksum: The 1's complement sum. In this basic example, if the sum was larger, it would require wraparound.
Receiver:
- Receives the data and computes checksum.
- Compares computed checksum with the received checksum.
Internet Checksum: Weak Protection
- Even though numbers have changed (bit flips), there can be no change in checksum.
Example:
Original Values:
`5 (0000000000000101)
6 (0000000000000110)`
Bit Flip Scenario:
`7 (0000000000000111)  // Bit flipped in first number
4 (0000000000000100)  // Bit flipped in second number`
Still result the same total checksum.

## Principles of Reliable Data Transfer
### Abstraction
- Provides a reliable service abstraction for application data transfer.
### Implementation
- Sender and receiver sides implement a reliable data transfer protocol.
- This ensures data is reliably transferred over an unreliable channel (network).
### Sender, Receiver Knowledge
- Sender and receiver don't inherently know each other's "state" (e.g., whether a message was received) unless communicated via a message.
### Reliable Data Transfer Protocol (rdt) Interfaces
- rdt_send(): Called from above (e.g., by application). Passes data to deliver to the receiver's upper layer.
- udt_send(): Called by rdt to transfer a packet over the unreliable channel to the receiver.
- rdt_rcv(): Called when a packet arrives on the receiver side of the channel.
- deliver_data(): Called by rdt to deliver data to the upper layer.
### rdt1.0: Reliable Transfer over a Reliable Channel
- Underlying channel is perfectly reliable:
  - No bit errors.
  - No loss of packets.
### rdt2.0: Channel with Bit Errors
- Underlying channel may flip bits in a packet.
- Checksum (e.g., Internet checksum) to detect bit errors.
How to Recover from Errors
- Acknowledgements (ACKs): Receiver explicitly tells the sender that a packet was received OK.
- Negative Acknowledgements (NAKs): Receiver explicitly tells the sender that a packet had errors.
- Sender Retransmission: Sender retransmits the packet on receipt of a NAK.
Stop-and-Wait
- The sender sends one packet, then waits for a receiver response.
### RDT 2.0 FSM Specifications
States and Transitions
- Wait for call from above:
  - Event: rdt_send(data)
  - Action: sndpkt = make_pkt(data, checksum); udt_send(sndpkt); state = Wait for ACK or NAK
- Wait for ACK or NAK:
  - Event: rdt_rcv(rcvpkt) && corrupt(rcvpkt)
  - Action: udt_send(NAK);
  - Event: rdt_rcv(rcvpkt) && notcorrupt(rcvpkt)
    - If isACK(rcvpkt): Action: state = Wait for call from above
    - If isNAK(rcvpkt): Action: udt_send(sndpkt);
Receiver FSM
- Wait for call from below:
  - Event: rdt_rcv(rcvpkt) && notcorrupt(rcvpkt)
  - Action: extract(rcvpkt, data); deliver_data(data); udt_send(ACK)
### rdt2.0: FSM specification
Sender
- State: Wait for call from above
- Event: rdt_send(data)
  - Action: snkpkt = make_pkt(data, checksum); udt_send(sndpkt);
  - Next State: Wait for ACK or NAK
- State: Wait for ACK or NAK
- Event: rdt_rcv (rcvpkt) && corrupt( rcvpkt )
  - Action: udt_send(NAK)
- Event: rdt_rcv (rcvpkt) && isACK ( rcvpkt )
  - Action: state = Wait for call from above
- Event: rdt_rcv ( rcvpkt ) && isNAK ( rcvpkt )
  - Action: udt_send(sndpkt)
Receiver
- State: Wait for call from below
- Event: rdt_rcv ( rcvpkt ) && notcorrupt ( rcvpkt )
  - Action: extract( rcvpkt,data ); deliver_data (data); udt_send (ACK)
### rdt2.0 Has a Fatal Flaw!
- What happens if the ACK/NAK is corrupted?
  - Sender doesn't know what happened at the receiver!
  - Can't just retransmit: possible duplicates.
- Handling duplicates:
  - Sender retransmits the current packet if the ACK/NAK is corrupted.
  - Sender adds a sequence number to each packet.
  - Receiver discards (doesn't deliver up) duplicate packets.
### rdt2.1: Sender, Handling Corrupted ACK/NAKs
- Seq # added to pkt.
- Must check if the received ACK/NAK is corrupted.
- Twice as many states.
  - State must "remember" whether the "expected" packet should have a sequence number of 0 or 1.
- Note: The receiver can not know if its last ACK/NAK was received OK at the sender.
### rdt2.2: A NAK-Free Protocol
- Same functionality as rdt2.1, using ACKs only.
- Instead of a NAK, the receiver sends an ACK for the last packet received OK.
  - The receiver must explicitly include the sequence number of the packet being ACKed.
- A duplicate ACK at the sender results in the same action as a NAK: retransmit the current packet.
### rdt3.0: Channels with Errors and Loss
- New channel assumption: The underlying channel can also lose packets (data, ACKs).
- Checksums, sequence numbers, ACKs, and retransmissions will be of help... but not quite enough.
- Approach: The sender waits a "reasonable" amount of time for an ACK.
  - Retransmits if no ACK is received in this time.
  - If a packet (or ACK) is just delayed (not lost):
    - Retransmission will be a duplicate, but sequence numbers already handle this!
    - The receiver must specify the sequence number of the packet being ACKed.
- Timeout: Use a countdown timer to interrupt after a "reasonable" amount of time.
### rdt3.0 in action
- Shows scenarios with no loss, packet loss, ACK loss, and premature timeout/delayed ACK.
### Performance of rdt3.0 (Stop-and-Wait)
- Example: 1 Gbps link, 15 ms prop. delay, 8000 bit packet.
[insert formula image]
- Time to transmit the packet into the channel.
rdt3.0 Protocol limits performance of underlying infrastructure (channel).
[insert formula image]
Where:
- U {sender} is the sender utilization
- L is the packet size in bits
- R is the transmission rate in bits per second
- RTT is the round trip time
### rdt3.0: Pipelined Protocols Operation
- Pipelining: The sender allows multiple "in-flight" yet-to-be-acknowledged packets.
  - The range of sequence numbers must be increased.
  - Buffering at the sender and/or receiver.
### Pipelining: Increased Utilization
- 3-packet pipelining increases utilization by a factor of 3.
[insert formula img]

### Go-Back-N (GBN)
- Sender:
  - A "window" of up to N consecutive, transmitted but un-ACKed packets.
  - k-bit sequence number in packet header.
  - Cumulative ACK: ACK(n) ACKs all packets up to, including sequence number n.
    - On receiving ACK(n): Move the window forward to begin at n+1.
- Timer for the oldest in-flight packet.
- timeout(n): Retransmit packet n and all higher sequence number packets in the window.

### Go-Back-N: Receiver
- ACK-only: Always send an ACK for a correctly-received packet so far, with the highest in-order sequence number.
  - May generate duplicate ACKs.
  - Only needs to remember rcv_base.
- On receipt of out-of-order packet:
  - Can discard (don't buffer) or buffer (implementation decision).
  - Re-ACK the packet with the highest in-order sequence number.
### Selective Repeat (SR)
- The receiver individually acknowledges all correctly-received packets.
  - Buffers packets, as needed, for eventual in-order delivery to the upper layer.
- The sender times-out/retransmits individually for un-ACKed packets.
  - The sender maintains a timer for each un-ACKed packet.
- Sender window
  - N consecutive sequence numbers.
  - Limits the sequence numbers of sent, un-ACKed packets.
### Selective Repeat: Sender, Receiver Windows
- Sender:
  - Data from above:
    - If the next available sequence number is in the window, send the packet.
  - timeout(n):
    - Resend packet n, restart the timer.
  - ACK(n) in [sendbase, sendbase+N]:
    - Mark packet n as received.
    - If n is the smallest un-ACKed packet, advance the window base to the next un-ACKed sequence number.
- Receiver:
  - Packet n in [rcvbase, rcvbase+N-1]:
    - Send ACK(n).
    - Out-of-order: Buffer.
    - In-order: Deliver (also deliver buffered, in-order packets), advance the window to the next not-yet-received packet.
  - Packet n in [rcvbase-N, rcvbase-1]:
    - ACK(n).
  - Otherwise:
    - Ignore.
### Selective Repeat: A Dilemma!
- Example: Sequence numbers 0, 1, 2, 3 (base 4 counting), window size = 3.
- The receiver can't distinguish between a retransmitted packet and a new packet in certain scenarios, leading to errors.
### Connection-Oriented Transport: TCP
- Segment Structure
- Reliable Data Transfer
- Flow Control
- Connection Management
- Principles of Congestion Control
- TCP Congestion Control
