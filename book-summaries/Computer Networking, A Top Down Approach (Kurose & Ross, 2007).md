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

[tbc]
