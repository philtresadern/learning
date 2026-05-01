# Computer Networks and the Internet
[tbc]

# Application Layer
[tbc]

# Transport Layer
Processes pass *messages* from the Application layer to the Transport layer via a socket. 
A Transport protocol then adds headers to the message, turning it into a *segment*, 
and it is scheduled for sending to the Network layer. In this way, the Transport layer
provides a logical communication between processes running on different hosts (as if
the two hosts were directly connected).

(In contrast, the Network layer provides a logical connection between hosts.)

Here, the authors use the analogy of someone (a multiplexer) who collects mail from all of the people
in a house (an office might have been a better analogy), bundles it up and gives it
to the postal service. The postal service then moves the bundle to another
office where someone else (the demultiplexer) takes the bundle and distributes it to
the people in that remote office.

Transport protocols are defined by the services they offer and guarantees that they make.
Different applications have different needs that might be better satisfied by different
protocols.

Internet Transport protocols include (and are dominated by) UDP and TCP.

## User Datagram Protocol (UDP)
UDP provides an almost minimal service between the Application and Network layers.
Effectively, it takes the message from the application process, 
adds a header and a checksum for error detection, and sends the segment on its way to the
Network layer protocol. As a result, it makes no guarantees about whether a message will be 
delivered at all, never mind in the right order or with no errors. (The checksum is available
for error detection by the application but that's about it.) This use case is appropriate for
synchronous messaging where some packet loss is acceptable (e.g., streaming media such as video
or voice) or where the delay involved in something more complex dominates the time taken to 
simply resend a small payload (e.g., in DNS).

Where data integrity and completeness is critical, UDP is no good.

The UDP header includes the source identifier (source IP address and port) and destination identifier
(destination IP address and port), though a UDP socket uses only the destination ID. Segments from
two different sources with the same destination will therefore be routed to the same UDP socket. 

In contrast, TCP uses a different socket for every (source, destination) pair. TCP also has a port
dedicated to receiving requests for connections. When a connection is accepted, the accepting host
creates a TCP socket identified by both the source and destination IDs. In some cases, sockets run in
their own process, though more modern hosts may use a single process and run sockets in their own
thread on the same process.

UDP does not use handshaking and is therefore considered *connectionless*.

## Principles of Reliable Data Transfer
UDP is known as an unreliable protocol because it offers no guarantees over whether segments will be
delivered at all, never mind in the right order. 
Now we consider what properties would be required of a Transport layer protocol that did offer such guarantees, even
when the Network layer protocol beneath it were unreliable.

The authors proceed to build such a protocol step-by-step using a sequence of Finite State Machines of
increasing complexity. Each new "version" introduces problems faced by the existing model, and a workable 
solution.

First is the perfectly reliable channel whereby packets are sent in order and received in order with
no feedback from the receiver. This is the ideal scenario, upon which we will build.

Next, acknowledgements (either a positive ACK or negative NAK) are added to check that a segment has
been received (or not) by the receiver before sending the next packet. This enables us to confirm that
a segment has been delivered intact (the receiver NAKs a corrupted segment, detected via a checksum in the 
segment header). If a NAK is received back, the sender resends the segment until an ACK is received.

To account for the possibility that an ACK (or NAK) might be lost from receiver to sender, the next
version adds a sequence number to packets (0 or 1 in this example, known as an alternating-bit protocol)
that indicate whether segments have
been received out-of-order. An ACK will only be sent if the received packet has the expected sequence 
number (based on the sequence number of the previously received segment).

The next model adds a timeout that triggers a resend in the event that an acknowledgement was not received
within some expected period of time. This introduces duplicate data segments, but the sequence numbering
handles this gracefully.

Because all of the above wait until an acknowledgement has been received before sending the next packet,
they can be unacceptably slow. The next version therefore implements a pipeline system whereby new  
messages are sent while waiting for the acknowledgement to old messages. Because we don't want to send
too much when we're uncertain whether messages are even getting through, there is a maximum number of
messages in the "pipeline" at any one time. The Go-Back-N protocol allows at most N unacknowledged messages
in the pipeline at once, though *all* unacknowledged messages are resent in the event of a timeout.

Because resending all messages in the pipeline can be wasteful (especially when N is large), the Selective
Repeat (SR) protocol allows for individual messages to be resent while the receiver buffers messages that
have been received (and acknowledged) correctly, even if they are out-of-order. (Segments can arrive out
of the expected order when travelling through a network with many possible paths from sender to receiver.)

The sequence numbering must be sufficient to handle the case where many messages are lost in sequence.
Otherwise, it can be possible for the received message to have the correct sequence number even though
it is many messages further on in the sequence.

## Transmission Control Protocol (TCP)

TCP is a connection-oriented transport protocol, meaning that a connection needs to be created between
sender and receiver before data transmission takes place. (UDP just sends the data and hopes for the best.)

The connection state resides entirely in the two end hosts and not in any of the intermediate hardware,
provides a full-duplex service (transmission in both directions simultaneously), and is always between
a single sender and a single receiver (no multicasting).

After establishing the connection with a *three-way handshake* (see below), 
data is put into the send buffer for transmission (i.e., transfer to
the network layer protocol). Segment size is limited by the *Maximum Segment Size (MSS)* parameter which
can be estimated from the largest link-layer frame - the *Maximum Transmission Unit (MTU)* - minus 
the size of TCP and IP headers.

### TCP Segment Structure

In addition to the application's payload data, a TCP segment contains a header (typically 20 bytes):

* addresses of the source (2 bytes) and destination (2 bytes)
* the sequence number (4 bytes): the index of the byte at which this segment starts
* an acknowledgement number (4 bytes): the index of the next byte expected by the receiver (or, alternatively, the first byte it is yet to receive)
* header length (4 bits)
* a flag field (6 bits) to indicate True/False statuses:
  * ACK: the acknowledgement field is valid
  * RST: used in set up and tear down (though not specified where or how in this book)
  * SYN: used to set up a connection
  * FIN: used to tear down a connection
  * PSH: Pass data to the upper layer immediately
  * URG: Sender has marked this as urgent
* the receive window (2 bytes)
* a checksum field (2 bytes)
* an urgent data pointer (2 bytes) which indicates the last byte of urgent data

and an options record (typically empty).

The random offsets in the sequence and acknowledgement numbers reduce the risk that an old packet still in
the system will be accepted as a packet in the current transmission, because its randomly offset sequence
number will not align with the (differently offset) sequence/acknowledgement numbers in the current
transmission. They are picked by the client and entered in the first packet.

Note that the receiver can add its updated acknowledgement number to data returned to the sender such that
it is *piggybacked* on the data segment without requiring a separate segment.

Note that when a packet is lost from sender to receiver, the receiver's acknowledgement will be for the 
missing packet regardless of whethr newer packets were successfully received. (These newer packets can be
discarded or buffered as desired - usually discarded.)

When determining an appropriate length for the timeout, TCP estimates the round trip time (RTT) and its
variability via an exponential weighted moving average (a linear blend between the current estimate and 
most recent sample). The timeout interval is then set to the estimated RTT plus four deviations.

[The authors then show some scenarios such as a lost packet, a timeout before acknowledgements are received,
and the loss of an acknowledgement after the next packet has already been sent.]

When a timeout occurs, the timeout interval is typically doubled before resending the packet, implementing
a form of congestion control.

*Fast retransmit* occurs when an acknowledgment is received for the same packet for a fourth time. The first
acknowledgment is expected, whereas the next three indicate that one packet was lost but three subsequent packets
were received. Because this suggests that the network is not congested, it seems safe to quickly retransmit
the lost packet rather than wait for the timeout.

### Flow Control

*Flow control* is a way to control the rate of data transmission on account of the specific receiver's ability to process it.
(It is therefore different from *congestion control* which controls the rate of data transmission on account
of the ability of the whole network to carry it.) 
Specifically, flow control is added to avoid overflowing the receiver's data buffer
by sending data quicker than the receiver can consume it. The sender maintains a *receive window* variable that 
is provided by the receiver to indicate the amount of space in its receive buffer. The sender then needs to 
ensure that the amount of unacknowledged data does not exceed this receive window by waiting to send any
data that would overflow the receiver's buffer. If the receive window falls to zero, the sender stops sending.
But, because the receiver will not send data unless there is data to send (or to acknowledge), it will not 
update the sender when the window becomes available. The sender therefore continues to send 1-byte packets
that the receiver can acknowledge, simply so that the sender can be updated on changes to the receive window.

### The Three-Way Handshake

1. The client sends the server a TCP segment (known as the *SYN segment*) with the SYN bit set to 1 and a random initial sequence number.
2. The server receives (we hope!) the SYN segment, allocates buffers and variables, and returns a *SYNACK segment* in which the SYN bit is set, the acknowledgement number is set to the client's initial sequence number plus one, and the server's initial sequence number set to a random value. This acknowledges the request to the client, telling the client that the server can 'hear' it.
3. The client receives the SYNACK segment, allocates its own buffers and variables, and returns the *ACK segment* in which the SYN bit is set back to 0, and the acknowledgement field is set to the server's initial sequence number plus one. This tells the server that the client can hear it. The connection is then established.

The same three-way handshake is used to tear down the connection, only using the *FIN* bit of the segment header 
to indicate the purpose of those segments.

These handshakes are effectively managed as a state machine where each host passes through a sequence of states
until they either reach the *ESTABLISHED* state or something goes wrong.

This TWH is prone to an attack known as *SYN flooding* whereby the attacker sends many requests to set up the connection
without acknowledging them. A naive server therefore keeps allocating buffers and variables in expectation of a
connection, possibly until the server's resources are exhausted. Using *SYN cookies* and delaying resource allocation
is a way to protect against this.

## Congestion Control

Congestion in a network (as opposed to in the buffers of a specific receiver, for which flow control is applied) 
results in delayed and lost packets being received (in either direction or both). It has consequences that include:

* large queueing delays as the packet arrival rate approaches the link capacity.
* senders must retransmit packets that were lot due to buffer overflow at the receiver, increasing the overall levels of traffic in the network.
* large delays due to congestion can result in packets being retransmitted unnecessarily, increasing congestion even more.
* packets dropped in a downstream link in the path mean that the links further upstream have wasted their time that could have been spent transmitting other packets.

*Network-assisted* congestion control depends on Network layer hardware sending signals (e.g., a *choke packet*)
to end hosts telling them to ease off. These choke packets - known as *resource management cells* - are interspersed
with data packets now and then, and contain bits to indicate the congestion status of the network.

*End-to-end* congestion control, however, happens at the Transport layer (e.g., in the TCP protocol itself) as 
an algorithmic detail. Specifically, it uses a *congestion window* variable, analogous to the 
*receive window* value used in flow control, to limit the rate of data sending. The de facto limit is the minimum of 
the receive window (determined by the receiver) and the congestion window (determined by the sender).

The congestion window is adjusted in response to acknowledgements being received (increasing the size of the window and
the data transmission limit) and packet loss detected via a timeout or duplicated ACKs (decreasing the size of the window
and the data transmission limit). Specifically, the window size is increased by a variable amount 
(*MSS* times *MSS*/*CongWin*) on every received acknowledgement
such that the limit increases linearly, and halves the size of the window on every detected packet loss such that
the limit decreases multiplicatively. The algorithm is thus known as *additive-increase, multiplicative decrease*.

The only exception is at the very start where the window is set to one *MSS* in which the algorithm imposes a *slow start*
but increases exponentially by increasing the congestion window size by one *MSS* on every acknowledgement (not every round trip).
As the rate of packet sending (and acknowledgement) increases, the rate of window increase also increases until packet loss
occurs and the linear increase is resumed.

[It looks to me as though] a loss event due to a timeout is treated differently, always resulting in reversion to 
the *slow start* and exponential regrowth rather than halving the size of the window. In some early algorithms, this
was also the response to a triple duplicate *ACK* but newer algorithms halve the window in response to this because
it is clear that some data is getting through at a reasonable rate so the network isn't totally congested.

In this way, transmission is *throttled* in response to data loss but allowed to return to normal levels if the network
permits it.

One consequence of the algorithm is that it shares the use of a single link among multiple connections fairly (which isn't
the case with UDP that can hog the bandwidth of a link). It also doesn't account for applications using multiple TCP connections
in parallel, but that's another story.

# The Network Layer

The next layer - Network - concerns how packets are *forwarded* and *routed* across a network from one end host to another via
a series of routers.

*Forwarding* is what happens within a single router, receiving a packet on one port and deciding which port it should go out on.
*Routing* is what happens within a network of routers, deciding the best series of routers to take out of the many possible
options.

Routers are little concerned with Transport and Application layers - they are a "Network and below" device.

Routers contain a *forwarding table* that maps packet destination addresses (in the Transport layer header) to output ports on the router.
(*Link-layer switches* use the header in the Link layer.) The forwarding table can be managed centrally or in a 
decentralized fashion.

A *Network Service Model* defines the guarantees that a service will provide (such as guaranteed delivery in a specific order) 
and some models make more guarantees than others. The *Constant bit rate (CBR) ATM network service* makes strong promises about
data transmission whereas the *Available bit rate (ABR) ATM network service* makes much weaker promises, and the 
*Internet Protocol (IP)* makes almost no promises whatsoever.

## Virtual Circuit and Datagram Networks

A *Virtual Circuit* establishes a route between two hosts that is fixed for the duration of the connection. (I.e., traffic
will flow through the same series of routers at all times.) The routers must therefore maintain information about the state
of the VC (packets coming in on a known VC need to go to a specific port that is on the VC). Specifically, when a VC is 
set up, its route is added to the forwarding table of every router on the path such that every router is aware of every VC
in which it is involved. When the VC is torn down, its entries are deleted from the forwarding table of the routers involved.

A *datagram network*, in contrast, maintain no state information about any connection and a router simply looks up the output port for
every destination address in its forwarding table. Because a one-to-one mapping between addresses and ports would be impractical, 
routers instead match only the *prefix* (the first N bits) of the address to a port on the understanding that addresses with 
similar prefixes are likely to be on networks that are physically (or logically) close. Where an address matches more than one
prefix, the longest (i.e., most specific) prefix is chosen as the match (the *longest prefix matching rule*).

In a datagram network, routing algorithms update forwarding tables every few minutes such that, over the course of a
transmission, packets may switch routes and therefore arrive in a different order to the order in which they were sent.

By analogy, VCs look like phone networks: complex routing between two simple endpoints (phones). In contrast, a datagram
networks uses simple routing to connect complex endpoints (client and server computers).

## What's Inside a Router?

A hardware router (which at one time might have just been a computer with multiple network cards) contains

* input ports where data comes in
* output ports where data goes out
* switch fabric that connects input ports to output ports, possibly with a buffer in between
* a routing processor that maintains the flow of data

Each input port consists of the Physical-, Link- and Network-layer infrastructure to get a packet from pulses on a wire
to a chunk of bits in memory. A physical port often handles input and output, and each port may reside on a *line card*
with its own infrastructure to speed up packet switching.

An input port should ideally be able to process its routing at *line speed* (i.e., in less time than it takes to receive a
packet at the input port) so that processing takes place before the next packet has been received. However, modern demands
require that this happens millions (or probably billions now) of times per second, leading to a number of briefly described
approaches to fast switching.

The switching fabric sends packets either through memory, or a shared bus, or an interconnection network, each with its
own properties in terms of routing speed.

## Where Does Queueing Occur?

Queues can form at either the input port (if data is received faster than it is processed) or the output port (if data
is processed faster than it can be sent, such as when packets arrive on many input ports simultaneously, all destined for
the same output port). When packets are queued at the output port, a *packet scheduler* chooses which packet to send next
and is therefore crucial for meeting quality-of-service guarantees.

The scheduler may take a *first-come-first-served (FCFS)* approach or something more complex such as *weighted fair queueing (WFQ)*.
Similarly, if a packet must be dropped the scheduler may choose the one just arriving (*drop-tail*) or another one in the
queue based on some criteria. Together, this *Active Queue Management (AQM)* keeps data flowing one way or another.
*Random Early Detection (RED)* maintains a weighted average length of the queue and drops packets at random with some 
probability that is based on the estimated "fullness" of the queue; the more full the queue, the more likely a packet
is to be dropped at random.

One consequence of queueing is *head-of-the-line (HOL) blocking* whereby a packets in an input queue are blocked when
the packet at the front of the queue cannot be transferred because the output queue is full, even though their 
output ports are free.

## The Internet Protocol (IP): Forwardindg and Addressing in the Internet

[tbc]
