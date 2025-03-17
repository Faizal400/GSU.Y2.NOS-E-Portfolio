# Transport Layer: Part 2

### Reliable Data Transfer (rdt) 3.0 in Action
- rdt3.0 builds upon rdt2.x to handle loss. It introduces timers to detect packet loss and retransmit packets.
- Includes scenarios for:
  - No loss
  - Packet loss
  - ACK loss
  - Premature timeout/delayed ACK
rdt3.0 sender and receiver actions under different scenarios:
- No Loss:
  - Sender sends pkt0, receiver receives pkt0 and sends ack0.
  - Sender sends pkt1, receiver receives pkt1 and sends ack1.
- Packet Loss:
  - pkt1 is lost (indicated by 'X').
  - Sender times out and resends pkt1.
  - Receiver detects duplicate pkt1 and resends ack1.
- ACK Loss:
  - ack1 is lost.
  - Sender times out and resends pkt1.
  - Receiver detects duplicate pkt1 and resends ack1.
- Premature Timeout/Delayed ACK:
  - pkt1 is sent, but the ACK is delayed.
  - Sender prematurely times out and resends pkt1.
  - Receiver detects duplicate pkt1 and resends ack1.

### Performance of rdt3.0 (Stop-and-Wait)
- Example: 1 Gbps link, 15 ms propagation delay, 8000 bit packet.
- Usender: Utilisation – fraction of time sender busy sending.
- Dtrans: Time to transmit packet into channel.
- ![image](https://github.com/user-attachments/assets/9eefce1e-c1db-407a-95f7-7e041a02ba1a)

Where:
- L = Packet size (8000 bits)
- R = Link bandwidth (1 Gbps)

### rdt3.0: Stop-and-Wait Operation Visualized
- First packet bit transmitted at t = 0.
- First packet bit arrives at the receiver after RTT/2.
- Last packet bit arrives, and ACK is sent after RTT/2 + L/R.
- ACK arrives, and next packet is sent after RTT + L/R.
### rdt3.0: Stop-and-Wait Operation - Utilization
![image](https://github.com/user-attachments/assets/a10f84b7-b2a6-4fc2-bc60-c9b952adfe1e)

Where:
- L = Packet size (8000 bits = .008 ms)
- R = Link bandwidth (1 Gbps)
- RTT = Round Trip Time (30 ms)
- rdt 3.0 protocol performance is poor.
- The protocol limits the performance of the underlying infrastructure (channel).
### rdt3.0: Pipelined Protocols Operation
- Pipelining: Sender allows multiple, “in-flight”, yet-to-be-acknowledged packets.
  - Range of sequence numbers must be increased.
  - Buffering at sender and/or receiver.
### Pipelining: Increased Utilization
- First packet bit transmitted at t = 0.
- Last bit transmitted after L/R.
- First packet bit arrives after RTT/2.
- Last bit of the second packet arrives, and ACK is sent.
- 3-packet pipelining increases utilization by a factor of 3!
![image](https://github.com/user-attachments/assets/c13a4a9f-a3ab-4c83-8352-5a64f2854729)

### Go-Back-N (GBN): Sender
- Sender: "Window" of up to N consecutive transmitted but un-ACKed packets.
- k-bit seq # in pkt header.
- Cumulative ACK: ACK(n): ACKs all packets up to, including seq # n.
  - On receiving ACK(n): move window forward to begin at n+1.
- Timer for the oldest in-flight packet.
- timeout(n): Retransmit packet n and all higher seq # packets in window.

### Go-Back-N (GBN): Receiver
- ACK-only: Always send ACK for the correctly-received packet so far, with the highest in-order seq #.
  - May generate duplicate ACKs.
  - Need only remember rcv_base.
- On receipt of out-of-order packet:
  - Can discard (don't buffer) or buffer: an implementation decision.
  - Re-ACK pkt with highest in-order seq #.
Receiver Sequence Number Space:
- rcv_base: Received and ACKed
- Out-of-order: Received but not ACKed
- Not received

### Go-Back-N in Action
- Example scenario demonstrating packet loss and retransmission.
- Sender window (N=4) slides as ACKs are received.
- Lost packet (pkt2) causes timeout and retransmission of pkt2 and subsequent packets.
- Duplicate ACKs are ignored by the sender after retransmission begins.

### Selective Repeat (SR)
- Receiver individually acknowledges all correctly received packets.
  - Buffers packets, as needed, for eventual in-order delivery to the upper layer.
- Sender times-out/retransmits individually for un-ACKed packets.
  - Sender maintains a timer for each un-ACKed pkt.
- Sender window:
  - N consecutive seq # s
  - Limits seq #s of sent, un-ACKed packets

### Selective Repeat: Sender, Receiver Windows
- Sender:
  - If the next available seq # is in the window, send packet
  - timeout(n): resend packet n, restart timer
  - ACK(n) in [sendbase, sendbase+N]: mark packet n as received. If n is smallest unACKed packet, advance window base to the next unACKed seq #
- Receiver:
  - packet n in [rcvbase, rcvbase+N-1]: send ACK(n); out-of-order: buffer; in-order: deliver (also deliver buffered, in-order packets), advance window to the next not-yet-received packet
  - packet n in [rcvbase - N, rcvbase - 1]: ACK(n)
  - otherwise: ignore

### Selective Repeat in Action
- Demonstrates buffering and individual acknowledgments.
- pkt2 is lost and retransmitted, while subsequent packets are buffered at the receiver.
- Once pkt2 is received, the buffered packets are delivered in order.

### Selective Repeat: A Dilemma!
- Problem: With a limited sequence number space, the receiver can't always distinguish between a retransmitted packet and a new packet.
- Example:
  - seq # s: 0, 1, 2, 3 (base 4 counting)
  - window size = 3
- Scenario (a): No problem. pkt3 is received.
- Scenario (b): Potential issue. pkt0 times out and is retransmitted. The receiver might incorrectly accept the retransmitted pkt0 as a new packet.
- Question: What relationship is needed between sequence # size and window size to avoid the problem in scenario (b)?
- Answer: The sequence number space must be large enough to avoid ambiguity. Specifically, the window size should be no more than half the size of the sequence number space.

### Outline of Topics
- Transport-layer services
- Multiplexing and demultiplexing
- Connectionless transport: UDP
- Principles of reliable data transfer
- Connection-oriented transport: TCP
  - segment structure
  - reliable data transfer
  - flow control
  - connection management
- Principles of congestion control
- TCP congestion control

### TCP: Overview
- RFCs: 793, 1122, 2018, 5681, 7323
- Cumulative ACKs
- Pipelining: TCP congestion and flow control set window size.
- Connection-oriented: Handshaking (exchange of control messages) initializes sender, receiver state before data exchange.
- Flow Controlled: Sender will not overwhelm receiver.
- Point-to-Point: One sender, one receiver.
- Reliable, in-order byte stream: No “message boundaries".
- Full-duplex data: Bi-directional data flow in the same connection.
- MSS: Maximum Segment Size

### TCP Segment Structure
- Source Port #: Port number of the sending application.
- Destination Port #: Port number of the receiving application.
- Sequence Number: Byte stream “number” of the first byte in the segment's data.
- Acknowledgement Number: Seq # of the next byte expected from the other side (cumulative ACK).
- Header Length: Length of the TCP header in 32-bit words.
- Flags: Control information (SYN, ACK, FIN, RST, URG, PSH).
- Receive Window (rwnd): Flow control; # bytes receiver willing to accept.
- Checksum: Internet checksum for error detection.
- Urgent Data Pointer: Indicates the end of urgent data.
- Options: Optional fields (e.g., maximum segment size).
- Data: Application data (variable length).

### TCP Sequence Numbers, ACKs
- Sequence Numbers: Byte stream "number" of the first byte in segment's data.
- Acknowledgements: Seq # of next byte expected from other side (cumulative ACK).
**Sender Sequence Number Space:**
- Outgoing segment from sender
- Sent ACKed
- Sent, not-yet ACKed ("in-flight")
- Usable but not yet sent
- Not usable
- Window Size N

Q: How receiver handles out-of-order segments? A: TCP spec doesn’t say, - up to implementor

### TCP Sequence Numbers, ACKs Example
- Simple telnet scenario where Host A types 'C' and Host B echoes it back.
- Host A sends "C" (Seq=42, ACK=79, data = ‘C’).
- Host B acknowledges receipt of 'C', echoes back 'C' (Seq=79, ACK=43, data = ‘C’).

### TCP Round Trip Time (RTT), Timeout
- Q: How to set TCP timeout value?
  - Longer than RTT, but RTT varies!
  - Too short: premature timeout, unnecessary retransmissions.
  - Too long: slow reaction to segment loss.
- Q: How to estimate RTT?
  - SampleRTT: Measured time from segment transmission until ACK receipt. Ignore retransmissions.
  - SampleRTT will vary, want estimated RTT "smoother". Average several recent measurements, not just the current SampleRTT.

### TCP Round Trip Time (RTT), Timeout - Estimation
![image](https://github.com/user-attachments/assets/972618d7-ca2a-4b2f-9d2c-0531b7c0c514)

- Exponential weighted moving average (EWMA)
- Influence of past samples decreases exponentially fast
- Typical value: α=0.125
### TCP Round Trip Time (RTT), Timeout - Timeout Interval
- Timeout Interval: EstimatedRTT plus "safety margin"
  - Large variation in EstimatedRTT: want a larger safety margin.
![image](https://github.com/user-attachments/assets/a2d1abb6-e44a-4d21-abbc-4b3919105049)

Where:
![image](https://github.com/user-attachments/assets/4a844a21-32cc-4231-ac89-a598543ad054)

- DevRTT: EWMA of SampleRTT deviation from EstimatedRTT (typically, β=0.25).

### TCP Sender (Simplified)
**Event: Data received from application**
- Create segment with seq #.
- Seq # is byte-stream number of the first data byte in the segment.
- Start timer if not already running. Think of the timer as for the oldest unACKed segment.
- Expiration interval: TimeOutInterval.
**Event: Timeout**
- Retransmit segment that caused timeout.
- Restart timer.
**Event: ACK received**
- If ACK acknowledges previously unACKed segments:
  - Update what is known to be ACKed.
  - Start timer if there are still unACKed segments.
### TCP Receiver: ACK Generation [RFC 5681]
**Event at Receiver:**
- Arrival of in-order segment with expected seq #. All data up to expected seq # already ACKed.
  - TCP receiver action: Delayed ACK. Wait up to 500ms for next segment. If no next segment, send ACK immediately.
- Arrival of in-order segment with expected seq #. One other segment has ACK pending.
  - TCP receiver action: Send single cumulative ACK, ACKing both in-order segments.
- Arrival of out-of-order segment higher-than-expect seq. #. Gap detected.
  - TCP receiver action: Immediately send duplicate ACK, indicating seq. # of next expected byte.
- Arrival of segment that partially or completely fills gap.
  - TCP receiver action: Immediate send ACK, provided that segment starts at lower end of gap

### TCP: Retransmission Scenarios
- Lost ACK Scenario: Host A sends data, Host B sends ACK, but the ACK is lost. Host A eventually times out and retransmits.
- Premature Timeout Scenario: Similar to the lost ACK scenario, but Host A's timer expires before the ACK arrives.

### TCP: Retransmission Scenarios and Cumulative ACKs
- Host A sends a sequence of data segments.
- Host B sends cumulative ACKs.
- A lost ACK is eventually covered by a subsequent cumulative ACK, avoiding unnecessary retransmissions.

### TCP Fast Retransmit
- Receipt of three duplicate ACKs indicates that three segments have been received after a missing segment – the lost segment is likely. So retransmit!
- If the sender receives 3 additional ACKs for the same data ("triple duplicate ACKs"), resend unACKed segment with the smallest seq #.
- Likely that unACKed segment is lost, so don’t wait for timeout.

### TCP Flow Control
- Q: What happens if the network layer delivers data faster than the application layer removes data from the socket buffers?
- Receiver-side buffering:
  - The receiver allocates a receive buffer (RcvBuffer) to store incoming data.
  - The TCP code places the payload from the IP datagram into the TCP socket buffer.
  - The application removes data from the TCP socket buffer.
- Flow control objective:
  - To prevent the sender from overwhelming the receiver's buffer.
  - The receiver controls the sender so that the sender will not overflow the receiver’s buffer by transmitting too much, too fast.

### TCP Flow Control Mechanism
- TCP receiver “advertises” free buffer space in the rwnd field in the TCP header.
  - RcvBuffer size is set via socket options (typical default is 4096 bytes). Many operating systems autoadjust RcvBuffer.
- The sender limits the amount of un-ACKed (“in-flight”) data to the received rwnd.
- Guarantees that the receive buffer will not overflow.

### TCP Connection Management
- Before exchanging data, sender/receiver "handshake":
  - Agree to establish connection (each knowing the other is willing to establish a connection).
  - Agree on connection parameters (e.g., starting seq #s).
- Connection State: ESTAB (Established)
- Connection Variables:
  - seq # client-to-server
  - seq # server-to-client
  - rcvBuffer size at server, client
- Socket creation:
  - Client: Socket clientSocket = new Socket("hostname", "port number");
  - Server: Socket connectionSocket = welcomeSocket.accept();

### Agreeing to Establish a Connection
- Q: Will a 2-way handshake always work in a network?
- Problems with 2-way handshake:
  - Variable delays
  - Retransmitted messages (e.g., req_conn(x)) due to message loss
  - Message reordering
  - Can't "see" the other side
- Scenarios:
  - No problem: A completes, data is exchanged
  - Problem: Half-open connection! (no client): Client terminates, server forgets x
  - **Problem**: dup data accepted!: Server terminates, accepts data x+1

### TCP 3-Way Handshake
- Step 1: SYN (Client -> Server)
  - Client chooses initial seq num, x.
  - Client sends TCP SYN msg (SYNbit=1, Seq=x) to server
  - Client state changes to SYNSENT.
- Step 2: SYN-ACK (Server -> Client)
  - Server receives SYN(x)
  - Server chooses initial seq num, y.
  - Server sends TCP SYNACK msg (SYNbit=1, Seq=y, ACKbit=1; ACKnum=x+1), acking SYN to client.
  - Server state changes to SYNRCVD.
- Step 3: ACK (Client -> Server)
  - Client receives SYNACK(x)
  - Client sends ACK for SYNACK (ACKbit=1, ACKnum=y+1) to server. This segment may contain client-to-server data
  - Client state changes to ESTAB (Established)
  - Server receives ACK(y), indicating client is live
  - Server state changes to ESTAB (Established)
### Closing a TCP Connection
- Client, server each close their side of the connection.
  - Send TCP segment with FIN bit = 1.
- Respond to received FIN with ACK.
  - On receiving FIN, ACK can be combined with own FIN.
- Simultaneous FIN exchanges can be handled.

### Principles of Congestion Control
- Congestion:
  - Informally: “too many sources sending too much data too fast for network to handle”
  - Manifestations: long delays (queueing in router buffers), packet loss (buffer overflow at routers)
  - Different from flow control!
- Congestion Control: Too many senders, sending too fast.
- Flow Control: One sender too fast for one receiver.
- A top-10 problem!

### Causes/Costs of Congestion: Scenario 1
Simplest scenario: two flows, one router, infinite buffers, input and output link capacity R, no retransmissions needed.
Maximum per-connection throughput: R/2
Q: What happens as arrival rate ![image](https://github.com/user-attachments/assets/79ba86c8-cbeb-411c-9d11-805612f634a3) approaches R/2?
A: Large delays as arrival rate ![image](https://github.com/user-attachments/assets/5ca3756d-ca4d-476b-907d-cd4bdf6eca4b) approaches capacity.

Throughout: As ![image](https://github.com/user-attachments/assets/b2656672-6766-4c40-80be-d502d00e849c) increases to R/2, the output ![image](https://github.com/user-attachments/assets/e0a631bc-bf22-43fc-af8e-d464f5663387) also increase to R/2

### Causes/Costs of Congestion: Scenario 2
- One router, finite buffers
- Sender retransmits lost, timed-out packet.
- Application-layer input = application-layer output: λin=λout
​- Transport-layer input includes retransmissions: λin′≥λin
​- Idealization: Perfect Knowledge
  - Sender sends only when router buffers are available.

### Causes/Costs of Congestion: Scenario 2 copy
- Host A send Data and also retransmits lost data
- Host B receives only data

### Causes/Costs of Congestion: Scenario 2 Timeout
- Realistic scenario: unneeded duplicates.
- Packets can be lost, dropped at the router due to full buffers – requiring retransmissions.
- But sender times out prematurely, sending two copies, both of which are delivered.
- “Costs” of congestion:
  - More work (retransmission) for a given receiver throughput.
  - Unneeded retransmissions: link carries multiple copies of a packet, decreasing the maximum achievable throughput.

### Causes/Costs of Congestion: Scenario 3
- Four senders, multi-hop paths, timeout/retransmit
Q: What happens as 
λin and λin′ increase?
A: As red λin′ increases, all arriving blue pkts at upper queue are dropped, blue throughput -> 0.

Another “cost” of congestion:
  - When a packet is dropped, any upstream transmission capacity and buffering used for that packet were wasted!

### Causes/Costs of Congestion: Insights
- Upstream transmission capacity/buffering is wasted for packets lost downstream.
- Delay increases as capacity is approached.
- Unneeded duplicates further decrease effective throughput.
- Loss/retransmission decreases effective throughput.
- Throughout can never exceed capacity.

### Approaches towards Congestion Control
- End-end congestion control:
  - No explicit feedback from network.
  - Congestion is inferred from observed loss and delay.
  - Approach taken by TCP.
- Network-assisted congestion control:
- Routers provide direct feedback to sending/receiving hosts with flows passing through the congested router.
- May indicate congestion level or explicitly set sending rate.

### TCP Congestion Control: AIMD
- AIMD (Additive Increase Multiplicative Decrease): Senders increase sending rate until packet loss (congestion) occurs, then decrease sending rate on loss event.
- AIMD Sawtooth Behavior: Probing for bandwidth
- Additive Increase: Increase sending rate by 1 maximum segment size (MSS) every RTT until loss is detected.
- Multiplicative Decrease: Cut sending rate in half at each loss event.

### TCP AIMD: more Multiplicative decrease detail:
- Cut in half on loss detected by triple duplicate ACK (TCP Reno).
- Cut to 1 MSS when loss is detected by timeout (TCP Tahoe).
- Why AIMD?
- AIMD – a distributed, asynchronous algorithm – has been shown to:
  - Optimize congested flow rates network-wide!
  - Have desirable stability properties.

### TCP Congestion Control: Details
- TCP sender limits transmission: LastByteSent - LastByteAcked < cwnd
  - cwnd: Congestion window.
  - Dynamically adjusted in response to observed network congestion (implementing TCP congestion control).
- TCP Sending Behavior:
  - Roughly: send cwnd bytes, wait RTT for ACKS, then send more bytes.
  - TCP rate ~ cwnd / RTT bytes/sec.

### TCP Slow Start
- When the connection begins, increase rate exponentially until the first loss event:
  - Initially, cwnd = 1 MSS.
  - Double cwnd every RTT (done by incrementing cwnd for every ACK received).
- Summary: the initial rate is slow, but ramps up exponentially fast.

### TCP: from slow start to congestion avoidance
- Q: when should the exponential increase switch to linear?
  - A: when cwnd gets to 1/2 of its value before the timeout.
- Implementation: variable ssthresh
- on loss event, ssthresh is set to 1/2 of cwnd just before the loss event

### Summary: TCP Congestion Control
State Diagram:
- Slow Start: cwnd increases exponentially until ssthresh is reached.
- Congestion Avoidance: cwnd increases linearly.
- Fast Recovery (TCP Reno): cwnd is adjusted based on duplicate ACKs.
- Actions upon Timeout: cwnd is reset to 1 MSS, ssthresh is set to cwnd/2, and slow start is initiated.

### TCP CUBIC
- Is there a better way than AIMD to “probe” for usable bandwidth?
- Insight/intuition:
  - Wmax: sending rate at which congestion loss was detected
  - congestion state of bottleneck link probably hasn’t changed much
  - after cutting rate/window in half on loss, initially ramp to Wmax faster, but then approach Wmax more slowly.
- K: point in time when TCP window size will reach Wmax
  - K itself is tuneable
  - larger increases when further away from K
  - smaller increases (cautious) when nearer K
- TCP CUBIC is the default in Linux, most popular TCP for popular Web servers.
- increase W as a function of the cube of the distance between the current time and K

### TCP and the congested “Bottleneck Link”
- TCP (classic, CUBIC) increase TCP’s sending rate until packet loss occurs at some router’s output: the bottleneck link.
- Understanding congestion: useful to focus on congested bottleneck link
- insight: increasing TCP sending rate will not increase end - end throughout with congested bottleneck
- insight: increasing TCP sending rate will increase measured RTT
- Goal: “keep the end - end pipe just full, but not fuller”

### Delay-based TCP congestion control
- Keeping sender - to - receiver pipe “just full enough, but no fuller”: keep bottleneck link busy transmitting, but avoid high delays/buffering.
- Delay-based approach:
  - RTTmin - minimum observed RTT (uncongested path)
  - uncongested throughput with congestion window cwnd is cwnd / RTTmin
  - if measured throughput “very close” to uncongested throughput increase cwnd linearly / since path not congested /
  - else if measured throughput “far below” uncongested throughput decrease cwnd linearly / since path is congested /
  - measured throughput = # bytes sent in last RTT interval

### Explicit Congestion Notification (ECN)
- TCP deployments often implement network-assisted congestion control.
- Two bits in the IP header (ToS field) are marked by a network router to indicate congestion.
  - The policy to determine marking is chosen by the network operator.
- The congestion indication is carried to the destination.
- The destination sets the ECE bit on the ACK segment to notify the sender of congestion.
- Involves both IP (IP header ECN bit marking) and TCP (TCP header C, E bit marking).

### TCP Fairness
- Fairness goal: if K TCP sessions share the same bottleneck link of bandwidth R, each should have an average rate of R/K.
- Q: Is TCP Fair?
  - A: Yes, under idealized assumptions:
    - Same RTT
    - Fixed number of sessions
    - Only in congestion avoidance

### Is TCP Fair? Fairness: must all network apps be “fair”? Fairness and UDP
- Multimedia apps often do not use TCP.
  - Do not want the rate to be throttled by congestion control.
- Instead, they use UDP:
  - Send audio/video at a constant rate, tolerate packet loss.
- There is no “Internet police” policing the use of congestion control.

### Fairness, parallel TCP connections
- An application can open multiple parallel connections between two hosts.
- Web browsers do this, e.g., link of rate R with 9 existing connections:
  - A new app asks for 1 TCP, gets rate R/10
  - A new app asks for 11 TCPs, gets R/2.

### Evolving transport-layer functionality
- TCP, UDP: principal transport protocols for 40 years
- different “flavors” of TCP developed, for specific scenarios:
- moving transport – layer functions to application layer, on top of UDP
- HTTP/3: QUIC

### Evolving Transport-Layer Functionality Scenario Challenges
- Long, fat pipes (large data transfers): Many packets “in flight”; loss shuts down the pipeline.
- Wireless networks: Loss due to noisy wireless links, mobility; TCP treats this as congestion loss.
- Long-delay links: Extremely long RTTs
 -Data center networks: Latency-sensitive background traffic flows, Low priority, “background” TCP flows

### QUIC: Quick UDP Internet Connections
- An application-layer protocol on top of UDP
- Increases performance of HTTP
- Deployed on many Google servers, apps (Chrome, mobile YouTube app)
- Adopts approaches studied in this chapter for connection establishment, error control, congestion control
  - Multiple application-level “streams” multiplexed over a single QUIC connection
  - Separate reliable data transfer, security
  - Common congestion control
  - Error and congestion control
  - Connection establishment

### QUIC: streams: parallelism, no HOL blocking
- Eliminates head-of-line blocking by using multiple independent streams within a single QUIC connection. This allows for better performance compared to HTTP/2 over TCP when packet loss occurs.

### Summary
- Principles behind transport layer services: multiplexing, demultiplexing, reliable data transfer, flow control, congestion control
- Instantiation and implementation in the Internet: UDP and TCP
