# Computer Networks and the Internet

The internet is a network of *end systems* or *hosts* - computers, smart phones, web cams, cars,
and so on. 
Hosts are connected via *packet switches* connected with *communication links*. 
Data is sent along a *path* or *route* between hosts in *packets* at a specific *transmission rate*.
Packet switches are typically either *routers* (that have an IP address) or 
*link-layer switches* (that do not have an IP address).
How the data is packaged and unpackaged along the way is determined by a variety of *protocols*,
implemented by hosts and routers, that specify "the format and order of messages
exchanged between two or more communicating entities, as well as the actions taken on the
transmission and/or receipt of a message or other event".
These protocols can be roughly organized in five layers
(though the OSI model uses seven): Application, Transport, Network, Link and Physical

## The Network Edge

Hosts can often be classified as *client* or *server* where the server is typically a more
powerful machine that serves many clients. Both the client and the server run their copy of the
application (or program) which are, by definition, *distributed applications*.

The closest router to a host (end system) is known as the *edge router* and is the first point
of contact with the "edge" of the network. Hosts are connected to their edge router via an
*access network*. Connections between nodes in the network can either by via *guided media*
(e.g., cables, analogous to trains) or *unguided media* (e.g., wireless, analogous to helicopters).

## The Network Core

Networks can be either *circuit-switching* or *packet-switching*.
In a circuit-switching network, the path is reserved and fixed for the duration of the transmission, 
and this path is called the *circuit*. Because transmission has idle times (or *silent periods*), 
circuit-switching networks are often considered to be wasteful or inefficient. A circuit can, however,
be shared among transmissions via *frequency-division multiplexing (FDM)* or *time-division multiplexing (TDM)*.

In a packet-switching network, the data is partitioned into packets that are sent independently across
the network such that different packets in the same transmission might arrive at their destination via
different routes (and therefore after different periods of time). Most packet switches use 
*store-and-forward transmission* such that the whole packet must be received before it can be sent to the
next node along the route. 
Each packet switch has an *output buffer/queue* that can incur *queueing delays* while it waits for the
output buffer to become free. If the buffer becomes completely full then *packet loss* might occur.

Packet switches allocate resources based on demand, known as *statistical multiplexing*, so that different
transmissions can share the links to and from the switch.

### ISPs and Internet Backbones

The internet is *a network of networks* organized into tiers.
* *Tier 1 ISPs* are the Big Boys (Sprint, Verizon, etc.), handling huge amounts of internet traffic, 
and are also known as *Internet backbone* networks. They cover many countries.
* *Tier 2 ISPs* have national or regional coverage and connect to only a few Tier 1 ISPs. Tier 2 ISPs
  are *customers* of the Tier 1 ISPs who are *providers* to the Tier 2 ISPs.
* *Lower-tier ISPs* are the customers of Tier 2 ISPs.

Two ISPs that are directly connected to each other are said to *peer* with each other.

## Delay, Loss and Throughput

Sending a packet between two hosts takes time. How much time depends on the *delays* and *losses* that
occur, impacting the *throughput*.

Node-to-node, a message will encounter the following delays:

* processing delay: after a packet arrives at a router, it must be processed so that the router can decide
  where to send it next. This processing delay is on the order of microseconds.
* queueing delay: having decided where the packet should go, the packet is put in a queue until it is ready
  to be transmitted. The time it spends in the queue - the *queueing delay* - depends on how many other
  packets are in the queue, waiting to be sent.
* transmission delay: this is the time between the first bit being put on the link and the last bit.
  It depends on how many bits need sending, and how quickly each bit can be transmitted (i.e., the *bandwidth*).
* propagation delay: this is the physical time it takes for a signal applied to one end of a link to be detected
  at the other end. In a fibre optic cable, for example, it takes @~0.3e-8 seconds to travel one metre.
  The propagation delay therefore depends on the medium and the length of the link.

The total *nodal delay* is the sum of these four delays, and the total host-to-host delay
is the sum of the nodal delays along the route.

As *traffic intensity* - average rate of bits arriving at the queue divided by the transmission rate - increases, 
queueing delays can become significant.

If a packet arrives to find the input queue full, the router might *drop* the packet and
*packet loss* occurs.

*traceroute* is a program that will display the node-by-node route taken by a packet from
one host to another, along with the nodal delay between each node, and is a vital tool in
debugging excessive delays in transmission.

*Throughput* is the rate at which bits can be received by host B from host A, and is typically 
limited by the *bottleneck link* (i.e., the slowest connection between two nodes on the route).

## Protocol Layers and Their Service Models

When taking a flight, for example, you pass through a number of stages 
(baggage check, passport control, gates, runway taxi) before takeoff.
After landing, you typically go through the same stages in reverse:
runway taxi, gates, passport control, baggage collection. 
Stacking the stages in layers, you take a U-shaped path from end to end.

The same thing applies in internet traffic. Protocols are organized in *layers*
and each layer comes with a *service model* that specifies what services a layer 
offers to the layer above. Protocols can be implemented in software or hardware,
and there are often alternative protocols to choose from at each layer.

These five layers transmit data in a unit that is named differently for each layer:

* The *application* layer transmits *messages*
* The *transport* layer transmits *segments*
* The *network* layer transmits *datagrams*
* The *link* layer transmits *frames*
* The *physical* layer transmits *bits*

As data travels *down* the stack, layer-specific headers are added to assist the
services operating at that layer. As the data travels back up the stack at the 
receiving end, the headers are stripped and used to control handling of the data
on its way.

Hosts operate at the application and transport layer; 
routers operate at the network layer;
switches operate at the link layer;
cables and wires operate at the physical layer.

## Networks Under Attack

Networks are fertile ground for bad guys who will come up with neverending ways
to make life difficult for you:

* *malware* (often *self-replicating*) carries malicious software that can infect a host
* a *botnet* takes over hosts and uses them for its own nefarious ends
* *viruses* spread via user interaction
* *worms* spread without any user interaction at all
* *trojan horses* disguise themselves as reputable software while doing nasty things in the background
* *denial-of-service (DoS)* attacks flood your host with traffic, rendering it unusable
* *distributed DoS* attacks recruit other hosts to flood your host with even more traffic
* *connection flooding* opens loads of connections to your host but leaves them open until no new connections can be made
* *packet sniffers* can listen in on your network traffic, looking for sensitive information
* *IP spoofing* can trick your system into thinking it is communicating with a trusted host
* *man-in-the-middle* attacks receive data on its route and modify it before sending it on in order to do damage

# Application Layer

The application layer is of most importance to the application developer, being closest to the developers
remit of work.

In a (simplified) network application, there is usually an *always-on* server that responds to communication
and one or more *sometimes-on* clients that initiate communication.

The application or program runs on each host in a *process* and it is processes, not programs, that communicate
with each other. 
On a single machine, inter-process communication is relatively easy;
over a network, it is much more complicated;
the aim of the layered services architecture is to abstract away the complexity and make it look and feel as though the client and server processes were running on the same host.

A process exchanges messages with the network via an interface called a *socket* which forms the passageway between
the application layer and the transport layer. The socket can also be considered the network's *Application
Programming Interface (API)*.

An application passing data through the socket to the transport layer might require guaranteed delivery, in which case
the transport layer service would have to provide *reliable data transfer* (e.g., TCP). 
Or it might not, opting to prioritise real-time delivery instead (e.g., a phone or video call) in which case 
the service (e.g., UDP) needn't worry about lost packets (a *loss-tolerant application*).

Other applications might have constraints on throughput, timing or security that must be accommodated 
by the transport layer service.

When exchanging messages between processes on hosts, the host is identified by its *IP address*
and the process by its *port number*. (Processes "listen* on a particular port to pick up messages that are
indended for them.)

Application layer protocols define how applications talk to each other without regard to how the data got from 
the sender to the receiver. In particular, they specify:

* the types of messages exchanged
* the message syntax
* the semantics (meaning) of the fields
* rules for processing and responding to messages

## The Web and HTTP

*HyperText Transfer Protocol (HTTP)* underpins the world wide web and determines
how a webpage gets from the server to the client (your web browser).

A *page* consists of *objects*, each of which has a URL with two parts:
the *host* (e.g., http://www.myschool.ac.uk) 
and the *path* (e.g., /myclass/class-photo.jpg).

When you load a web page, the client sends an HTTP *request* to the server
and the server sends an HTTP *response*, containing the data requested, to 
the client.

HTTP uses TCP as its transport protocol of choice, passing the request through
the client's socket to TCP at the transport layer and from the server's transport
layer to the server's web server application via the server's socket. 
Sending the response does the same in reverse.

HTTP servers retain no information about the clients and is therefore said to be 
a *stateless protocol*.

HTTP can also use *non-persistent connections* (where the TCP connection serves a 
single request/response transaction at a time) or *persistent connections* (where the
TCP connection serves many transactions before closing).

When using a non-persistent connection, the client might request a web page and, having
received the HTML the TCP connection is closed. If the HTML links to several images
or other documents, each object will be requested and received in turn with a new
TCP connection opened and closed for every object. 
Many TCP connections can, however, be operating in parallel for efficiency.

When using a persistent connection, request/response transactions occur many times
after a single initiating handshake, and the server typically closes the TCP connection
after some period of inactivity.

### HTTP Message Format

A typical HTTP message looks like this:

```
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agent: Mozilla/4.0
Accept-language: fr
```

The first line is the *request line* and the remaining lines are *header lines*.
For some methods (e.g., *POST*) there would be a blank line followed by the body
of the message.

The header lines specify:

* which host the data is requested from which,
  although implicit in the TCP connection used to send the request,
  is included in the message for other uses (e.g,. Web proxy servers).
* whether to close the connection after serving the request
* what agent (client) will be presenting the data
* what language is preferred

Variables can also be included in the URL of a GET request, 
using ? and = to specify key-value pairs, e.g.,
GET /car-list?make=ford&model=fiesta

The response typically takes the form:

```
HTTP/1.1 200 OK
Connection: close
Date: Thu, 07 Jul 2007 12:00:15 GMT
Server: Apache/1.3.0 (Unix)
Last-Modified: Sun, 6 May 2007 09:23:45 GMT
Content-Length: 6821
Content-Type: text/html

(data data data data data data ...)
```

The first line is the *status line* that gives a code corresponding to
the outcome of the request ("200 OK" in this case).
There are then several *header lines* including:

* Date: when the response was created
* Server: analogous to the User-Agent field in the request
* Last-Modified: when the data was last changed, which is crucial when caching
* Content-Length: number of bytes in the data
* Content-Type: how the data should be interpreted (independently of the file extension)

Common statuses include 200 OK, 301 Moved Permanently, 400 Bad Request, 404 Not Found and 505 HTTP Version Not Supported.

### Cookies

HTTP is stateless such that no information about the client is stored on the server.
Where user state would be beneficial, the server might respond to the first request 
with an additional `Set-Cookie: 3141` header line in its response. 
The client browser then adds the cookie number 3141 to its file for that user and
server, and passes the cookie number into all subsequent requests to that server.
The server application can then maintain a record of that user's interactions 
via a backend database for a better user experience.

### Web Caching

Where the same data are repeatedly requested from a server (which we'll refer to as
an *origin server*), this incurs inefficiency.
This inefficiency can be overcome by placing a *Web cache* or *proxy server* in between
clients and the origin server. Clients send their request to the proxy server which
maintains a cache of recently requested. If the data are in the cache, the proxy server
responds to the client's request. If the data are not in the cache, the proxy server
requests the data from the origin server, caches it, and responds to the client's request.

Proxy servers are typically purchased by large ISPs or organizations to which many clients
connect. For example, a university may host its own proxy servers through which requests 
from its internal network are filtered. The use of the cache reduces traffic on the 
outbound connection to the internet which saves money in the long run.

When a request comes in from a client, the proxy server may a *conditional GET* 
to the origin server to check that its cached data are still valid.
In the conditional GET, an additional field - `If-modified-since` containing the
`Last-modified` value of the data when it was last received - is included in the
header. The origin server then checks the modification date of the data requested
and, if it has not changed since it was cached, returns a tiny 304 response telling
the proxy server that it is safe to respond to the client with the cached data.

## File Transfer: FTP

Controlling the file system of a remote machine often uses FTP (or its secure version,
*SFTP*) which is similar to HTTP but with some important differences:

* FTP uses two TCP connections in parallel: The persistent *command connection*
  and the non-persistent *data connection*. This separation of commands from
  data is called *out-of-band* whereas protocols that combine commands and data
  (e.g., HTTP, SMTP) are *in-band*.
* The command connection is used to transmit user credentials, and FTP maintains
  state about the connected user (e.g., their current directory). This limits
  concurrency in FTP unlike with stateless protocols such as HTTP.

FTP messages, like HTTP, are in human-readable ASCII. 
Requests typically start with a four letter command (USER, PASS, LIST, RETR, SEND) with
zero or more arguments.
Responses typically start with a three-digit code with explanatory text following.

## Electronic Mail

The electronic mail (e-mail) system is composed of *user agents*, *mail servers* and the 
*Simple Mail Transfer Protocol (SMTP)*.

Every user (email address) has a mailbox on their mail server that manages the emails sent to
and from the user. In particular, it puts messages in a queue for sending or receiving thus
handling data connection problems between mail servers (asynchronous communication).

Like HTTP and FTP, SMTP is text based but in the case of SMTP any binary data must also be 
converted to 7-bit ASCII.

The important part is how the mail servers communicate (directly) with each other via SMTP 
where the sender acts as client and the recipient acts as server.

After the TCP connection is initialized, 
communication starts with an introductory handshake

```
S: 220 hamburger.edu
C: HELO crepes.fr
S: 250 Hello crepes.fr, pleased to meet you
```

followed by a series of back-and forth exchanges.
First, the client announces that it has mail to send

```
C: MAIL FROM: <alice@crepes.fr>
S: 250 alice@crepes.fr ... Sender ok
```

and to whom the mail is directed

```
C: RCPT TO: <bob@hamburger.edu>
S: 250 bob@hamburger.edu ... Recipient ok
```

and that it has data to send

```
C: DATA
S: 354 Enter mail, end with "." on a line by itself
```

and then the body of the email

```
C: Do you like ketchup?
C: How about pickles?
C: .
S: 250 Message accepted for delivery
```

and then ends the exchange
354 Enter mail, end with "." on a line by itself
```
C: QUIT
S: 221 hamburger.edu closing connection
```
(Apart from my "S:" and "C:" prefixes, this is what is sent, verbatim.)

SMTP uses persistent connections such that many messages can be sent in one TCP connection.
SMTP is also considered a *push protocol*, initiated by the sender, 
rather than a *pull protocol* initiated by the recipient (as in HTTP, for example).
As a result, SMTP is used to push the message from the sender's user agent
to the sender's mail server, and also to push the message from the sender's mail
server to the recipient's mail server. Because the recipient typically *pulls* 
mail from their server, different Mail Access Protocols are used for this (see below).

The user agent, when drafting an email will add header lines to the message including
the sender, the recipient and a subject line (none of which correspond to anything
in SMTP, and some of which - the sender, for example - are presumably used to spoof 
email addresses).

When binary data are attached to the email using 
*Multipurpose Internet Mail Exchange* extensions,
additional headers such as `Content-Type: image/jpeh` and 
`Content-Transfer-Encoding: base64` indicate
how the textual data should be turned back into binary, and what the binary data
represent (e.g., a JPEG that could be displayed by the user agent).

The receiving mail server also attaches a header of its own to messages, 
showing from which mail server the message came and the path the message
took from sender to receiver (intermediate mail servers may be involved).

Using mail servers rather than sending directly between user agents makes the 
process more robust by enabling message queueing on servers that are always on
(unlike the user agents) and facilitates asynchronous communication at the
heart of email.

### Mail Access Protocols

Pulling mail from the recipient's mail server typically uses one of the
two most popular protocols: *Post Office Protocol v3 (POP3)* or 
*Internet Mail Access Protocol (IMAP)*.

POP3 is the simpler of the two and follows three stages: 
authorization (agent supplies username and password), 
transaction (mail server responds to requests from the agent), 
and update (delete messages from the server if requested).

The commands used in the transaction phase are simply `list`, `retr`, `dele` and `quit`
with obvious interpretations. POP3 agents can be set up to mark messages for deletion
upon retrieval or to keep them on the server for posterity. Messages are deleted by
the server after the transaction phase is complete.

Note that deleteing messages means that they will exist only on the agent that
received them and will be unavailable to other agents.

The more modern IMAP protocol is much more powerful and allows users to organize emails 
(e.g., into folders) on the server itself rather than just on their agent. This
requires IMAP to store user information across sessions.)

## DNS - The Internet's Directory Service

Internet hosts are identified by their unique *IP address*: four numbers in the
range 0-255, separated by a period, such as 121.7.13.187. Since these are hard to 
remember, we need a system for retrieving this hard-to-remember number from an
easy-to-remember name (such as "google.com" or "bbc.co.uk"). This is the purpose
of the *Domain Name System*.

DNS is "a distributed database implemented in a hierarchy of *DNS servers*"
and also an application layer protocol that allows hosts to query the database.

Because this system is used by all other protocols (HTTP, FTP, SMTP, and many more)
it is considered a core function of the internet rather than an application with which
a user might interact.

DNS also provides:

* host aliasing: a host with the hostname server1.google.com can have the aliases
  www.google.com and google.com. DNS can map memorable aliases to complex hostnames
  as part of its resolution.
* mail server aliasing: in the same way, an alias can be mapped to a mail server, even
  when the alias is used by other servers in the same network.
* load distribution: an alias can be mapped to many hostnames in rotation such that
  requests are sent to (and handled by) different hosts in the network, thus spreading
  the load rather than directing all traffic to a single host server.

### How DNS Works

To handle the scale of the modern internet, DNS must be distributed and in a scalable way;
it cannot be run on a single server (or cluster of servers).

Instead, there are four types of DNS server:

* The Local DNS server is the first point of contact to which the DNS request, and is often
  on the same network as the requesting host.
* An authoritative DNS server knows the addresses of all the hosts for a given domain
  (e.g., visionomy.com). Large institutions host their own; everyone else uses their ISP's.
* A Top Level Domain server knows the addresses of all the authoritative DNS servers
  for a given TLD (e.g,. .com)
* A Root DNS server knows the addresses of all the TLD servers.

A DNS request then proceeds roughly as follows:

* The host makes the request to its Local DNS server.
* The Local DNS server sends the request to a Root server who responds with the address of a TLD server.
* The Local DNS server then sends the request to the TLD server who responds with the address of an authoritative server
* The Local DNS server then sends the request to the authoritative server who responds with the address of the desired host.

As usual where lots of requests/responses are made, caching is used extensively to return answers that
an intermediate DNS server already knows. Cached queries are usually discarded after a period of time
(often two days).

### DNS Records and Messages

The DNS database is composed of *resource records (RRs)* that are a four-tuple:
(Name, Value, Type, TTL).

* If Type=A (an *A Record*), Name is the hostname and Value is the IP Address..
* If Type=NS (an *NS Record*), Name is a domain and Value is the address of an authoritative name server.
* If Type=CNAME (a *CNAME Record*), Name is the (easy-to-remember) alias
  and Value is the (easy-to-forget) canonical hostname.
* If Type=MX (an *MX Record*), Name is the (easy-to-remember) alias
  and Value is the (easy-to-forget) canonical mail server hostname.

(TTL is the *time-to-live* after which time a cached record should be deleted.)

Queries and responses use the same message format (details omitted here) that indicates (among other things):

* whether the message is a query or a response
* what kinds of address (A, NS, CNAME, MX) is being queried and for whom
* the answer (or answers, when cached data is available) to the query

The nslookup tool allows you to query the DNS database yourself if you feel curious.

Adding records into the DNS database is done by a *registrar* to whom you pass your
records and they add them after checking that the data are correct.

## Peer-to-Peer Applications

(Of only passing interest.)

## Socket Programming with TCP/UDP

(Java Examples using TCP libraries to exchange data.)

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

* port number of the source (2 bytes) and destination (2 bytes)
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

Note that the source and destination fields are port numbers only - no IP address, presumably because the IP
address will be added by, well, IP in the Network layer. (Makes sense.)

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

## The Internet Protocol (IP): Forwarding and Addressing in the Internet

There are two versions of the Internet Protocol: IPv4 and IPv6.

An IPv4 datagram contains:

* the version number (4 bits): 4, always, to distinguish it from IPv6
* header length (4b): the datagram can include options, though these are usually empty which gives a header length of 20 bytes
* Type of Service (TOS) (8b): what kind of data is being sent and whether it has specific requirements on speed and integrity
* datagram length (16b): total number of bytes of data in the datagram (header + payload)
* identifier (16b), flags (3b), fragmentation offset (13b): indicate whether a datagram was broken up (fragmented) and the order of the fragments
* Time-to-live (TTL) (8b): number of hops before the datagram is dropped
* protocol (8b): the Transport layer protocol to which the data should be delivered
* header checksum (16b): a value to indicate errors in the datagram header only
* source IP address (32b): the IP address of the source host
* destination IP address (32b): IP address of the destination host
* options (variable): not really considered here
* data payload: the Transport layer segment to be delivered

Because IP datagrams must be transmitted over a sequence of Link layer connections, the largest datagram that can be sent
is limited in size by the smallest link layer frame along the route. If a datagram exceeds this Maximum Transmission Unit (MTU)
then it must be broken up (fragmented), sent in pieces, and reassembled at the Network layer of the receiving end.
Each datagram therefore uses the identifier field to uniquely identify a datagram; the flag field to indicate whether a datagram
is part of a larger, fragmented datagram; and the fragmentation offset field to indicate the fragment's position in the sequence of
fragments. The last fragment in a datagram sequence takes a flag value of 0 whereas preceding fragments take the value 1.
An unfragmented datagram has a flag of 0 but also a fragmentation offset of 0 (whereas the final fragment would have a flag of 0
and a nonzero offset). Fragmentation is useful but puts a burden on routers, and can also be a source of malicious behaviour.

### IPv4 Addressing

An IP address is technically associated with an *interface* rather than with the *host* or *router* containing that interface.
The interface is the boundary between the physical connection and the logical host. (An ethernet port would be one interface; a
wireless connection another; hosts typically use only one interface.)

IP addresses come in *dotted-decimal notation* with four 8-bit numbers separated by dots, e.g., 213.64.127.12, and every 
interface has its own unique IP address.

A router has one interface for every physical connection such that a router with four physical sockets will have four interfaces
and therefore four separate IP addresses (one per interface).

A group of interfaces connected together form a *subnet* and all have IP addresses that start with the same n<32 bits (where
the first n bits constitute the *prefix*). The number n is denoted by "/n" after an IP address (the *subnet mask*), 
e.g., 213.64.127.0/24 indicates the subnet starting with the 24 bits 213.64.127 where the last 8 bits indicate the host in that
subnet.

"To determine the subnets in a network, detach each interface from its host or router, creating islands of isolated networks,
with interfaces terminating the end points of the isolated network. Each of these isolated networks is a subnet."

Each of a routers input/output ports will therefore belong to a different subnet, hence why each port has its own unique IP address.

Addresses are assigned using *Classless Interdomain Routing (CIDR)* (pronounced "cider") whereby an ISP is allocated a range
of addresses with a common prefix (like a subnet) and every organization whom the ISP serves gets the same prefix. The ISP is 
then responsible for divvying up the pool among organizations. (Each organization might then be allocated a smaller pool of IP
addresses by the ISP, and it is the organization's responsibility for divvying up that pool among its routers and hosts.)

If an ISP acquires another ISP, forwarding rules can effectively merge the two ISPs as far as the outside world is concerned.

(*Classful addressing* assigned prefixes in groups of 8, 16 or 24 bits, meaning that the next biggest pool available was
255 times bigger than the one before. This coarse granularity meant that a lot of IP addresses were going unused. CIDR enables
you to get a pool that is only twice as big when needed.)

The Internet Corporation for Assigned Names and Numbers (ICANN) assigns IP address pools to organizations.

When connecting to a subnet for the first time, a host must be allocated an IP address to access the network. This was once
done manually before the *Dynamic Host Configuration Protocol (DHCP)* came along. Under DHCP:

1. a host connects to the network and broadcasts a *DHCP server discover message* on special broadcast IP address 255.255.255.255 (sent to all hosts) using UDP port 67 with source address 0.0.0.0 (because it has no IP address yet)
2. each DHCP server on the network receives the request and broadcasts a *DHCP server offer* containing the IP address of the DHCP server, an offered IP address for the host, a transaction ID and a lease time after which the IP address will have to be renewed
3. the host will choose from the one or more offers and broadcast a *DHCP request* for the chosen IP address along with the IP address of the corresponding DHCP server (which, presumably, signals to the other DHCP servers that their offers are declined, and also disambiguates if two DHCP servers offer the same IP address)
4. the selected DHCP server sends a *DHCP ACK* confirming that the IP address has been assigned.

So far, we have assumed that every host has a unique IP address, though with only 4 billion to choose from this could easily now
be exhausted. With many hosts/interfaces will reside on a Local Area Network, connected to one router that provides
Internet services, *network address translation (NAT)* provides a simple way for many hosts, each with its own *private IP address*
to share a single *public IP address* that is assigned to the router. (A private address lives on one of the three subdomains
10.0.0.0/8, 172.16.0.0/12 or 192.168.0.0/16.)

On the basis that each host will only use a few ports, with NAT the router maps a (source host's private IP, source port) tuple 
to a (router's public IP, randomly chosen router port) tuple and uses this latter address as the source for the packet from the router.
When the destination host replies with (router's public IP, router port) as the destination, the router reverses the mapping
back to (source host's private IP, source port) and sends the response on its way to the host. In this way, the router turns
"many hosts, few ports per host" into "one router, many ports per router" to demultiplex the incoming data.

Critics of NAT say that a router shouldn't mess with Transport layer data (ports), but NAT has become a convenient way to 
manage LANs without exhausting IP address pools. Peer-to-Peer also does not play well with NAT because a TCP connection is
between a labelled source and a labelled destination, and NAT messes with the source IDs (IP address and port). UPnP patches this
by providing a way for hosts to request a specific mapping that they can use to establish the TCP connection instead.

### Internet Control Message Protocol (ICMP)

A special type of message sent at the Network layer is the *Control Message*, used to indicate conditions and errors that may
arise during transmission. Each ICMP packet contains one of 13 (type, code) pairs, each associated with a specific message
(e.g., "Destination host unreachable"). A common use of ICMP is in the "ping" program that sends an (8,0) code to the destination
host, requesting an echo response. The destination host might then send back a (0,0) echo response to the source, indicating that
it is reachable.

Another use of ICMP is in the Traceroute program that sends out a sequence of IP datagrams, each containing a UDP segment with an
unlikely port number, where each datagram has a TTL value one greater than the last. (The first datagram has a TTL=1.) Each router
along the path to the destination host therefore returns the packet (because the TTL is decremented at every hop until it reaches
zero) until the packet that actually reaches the destination host who returns a (3,3) ICMP packet corresponding to "destination
port unreachable". At this point, Traceroute knows it has found the endpoint of the route and stops transmitting packets. The returned
packets include information containing the IP address of each router that returned the packet, and the round trip time to each
router.

### IPv6

Based on experience with IPv4 and the limited number of IP addresses, IPv6 is taking over. IPv6 increases the bit-length of addresses
to 128-bit (enough for every grain of sand to have an IP address) and ditches some of the less well-liked fields in the header.

In particular, fragmentation is no longer a thing - a datagram that is too big is returned with a "too big" message (via ICMPv6)
and the sending host is responsible for slicing it up. Checksums are also gone, again pushing error detection up the stack
to higher layers. All of this reduces the load on routers that can focus on the task of routing.

During the transition from IPv4 to IPv6, one approach was for devices to implement a *dual-stack* whereby datagrams could be sent
and received in either IPv4 or IPv6, and every interface is required to have an address in both forms. In the case where two
IPv6 nodes are linked by a route containing only IPv4 nodes, IPv4 must be used to traverse the *tunnel*. In this case, the IPv6
node preceding the first IPv4-only node wraps the entire IPv6 datagram into an IPv4 datagram and sends it on its way. The first
IPv6 node after the last IPv4 node in the tunnel then extracts the IPv6 datagram and sends it on its way.

Finally, a protocol called IPsec (used in VPNs) sits between TCP/UDP and IP, securing data and the connection between the two hosts. 
In a nutshell TCP/UDP passes its segment to IPsec that encrypts it and wraps it with a header, and passes this datagram to IP which wraps it 
with the usual headers. At the receiving end, IPsec reverses the process to extract the sensitive data. (This will be covered in more
detail later.)

## Routing Algorithms

The other main function performed by a router is, unsurprisingly, determining the best route between the two hosts. 

Hosts are attached to the *default router* (or *gateway* or *first-hop router*): the sending host to the *source router* and the receiving host to the *destination router*. 

The network in between hosts is represented as a graph with Nodes (routers) and Edges (links). Each edge is associated with a cost (whose definition is of no concern here) and nodes *x* and *y* are said to be *neighbours* if (*x*,*y*) is in the set of edges of the graph. The purpose of the routing algorithm is to find the path from source to destination that minimizes the sum of the edge costs, known as the *least-cost path*.

Routing algorithms come in two flavours:

* in a *global routing algorithm*, often referred to as a *link-state (LS) algorithm* knowledge of the entire graph is required to compute the least-cost path. This means that it can be computed on any node that has this information.
* in a *decentralized routing algorithm* each node computes the next step based on very localized information about the current path.

Algorithms can also be categorized as *static* (recomputed infrequently, perhaps manually) or *dynamic* (recomputed frequently, triggered automatically, susceptible to loops and oscillation), and as *load-sensitive* (link costs increase with network traffic) or *load-insensitive* (link costs are not related to current traffic loads).

### The Link-State (LS) Algorithm

In a *Link-State (LS)* algorithm, the source node knows the shortest distance (and corresponding route) to *every* other node in the network. Since this includes the destination node, it knows how to get there with the lowest cost. One example of an LS algorithm is Dijsktra's algorithm whereby nodes are added to the neighbourhood of the source node one at a time with the distance-to-source for every node updated at every iteration, where there are as many iterations as there are nodes. 

One downside of an LS algoithm (and many other algorithms) is the risk of oscillations as the shortest path is updated repeatedly, switching between two or more paths that are shortest at any given time. This is especially the case where link costs are associated with traffic load that the algorithm controls. This can be partly countered by running the algorithm at random times so that nodes are unsynchronized, though they have a nasty habit of synchronizing themselves.

### The Distance Vector (DV) Algorithm

In an implementation of the *Distance Vector (DV) Algorithm* (e.g., RIP, BGP, ISO IDRP or Novell IPX) each node maintains a state that includes: the estimated minimum distance from itself to every other node (the distance vector, DV) and the DV for each of its direct neighbours. Each node sends its DV to each of its neighbours, each of whom uses it to update their estimate of the shortest distance. If that estimate changes, then they send their updated DV to each of their neighbours (and so on and so forth). Updates needn't be sent synchronously and can be applied at any time, and DVs settle into a steady ("quiescent") state -- equal to that found by an LS algorithm --after a certain number of iterations. No more updates will be applied until one of the costs to an immediate neighbour changes, at which point updates will be delivered once again. Sending a packet along the least-cost path then requires each node to know only which is the next-hop node on the least cost path, which is available from its DV.

Changes in the costs to immediate neighbours can propagate through a network very quickly (especially if the cost goes down) but can also take many iterations to propagate fully (e.g., if there is a dramatic increase in cost). In the latter case, a packet can bounce between two neighbouring nodes for a long time since each is convinced that the other is on the shortest path. One approach to avoiding this situation is to "poison" the DV of the originating node such that sending the packet back is considered very costly. Although this *poisoned reverse" can help, it does not work under all circumstances.

These two classes of algorithm (LS and DV) are essentially the only ones in use in the Internet today. Other algorithms have been proposed but never caught on.

### Hierarchical Routing

As the number of nodes in a network grows, the resource requirements of these algorithms becomes prohibitive. Furthermore, the organisation that owns a router might want *administrative autonomy* over how that router is managed (e.g., what routing algorithm it uses).

Clusters of nodes belonging to the same organisation are therefore grouped together in an *Autonomous System (AS)* that can comprise one or more subnets (in the CIDR addressing sense). Routers belonging to the same AS must share the same *intra-autonomous system routing protocol* to pass packets among each other. Any router within an AS that connects to routers in other AS's is known as a *gateway router*, and gateway routers must share the same *inter-autonomous system routing protocol* (which happens to be BGP4) to pass packets between AS's. Routers within an AS must therefore maintain knowledge of to which gateway router in their AS they need to send packets in order to reach a given destination. Where more than one gateway router lies on the path to a given destination, the "nearest" one (with the smallest cost) is usually chosen (known as *hot-potato routing*).

Routers within an AS pass routing information among each other but not outside the AS. As well as sharing routing information among the other routers in their AS, gateway routers pass routing information to gateway routers in other ASs though they needn't share routing information about all destinations (for economic rather than technical reasons).

## Routing in the Internet

Intra-AS routing protocols -- also known as *interior gateway protocols* -- move packets within an AS, and are dominated by the *Routing Information Protocol (RIP)* and *Open Shortest Path First (OSPF)* protocol. 

### Intra-AS Routing in the Internet: RIP

RIP is a DV algorithm that maintains the distance vector from each node to other *subnets* (rather than nodes) in the AS where each link is given a cost of 1 (counting "hops"), and the algorithm is limited to only 15 "hops". Neighbours exchange a *RIP response message* (or *RIP advertisement*) approximately every 30 seconds, containing distance information to no more than 25 subnets. If no advertisement is received from a neighbour after 180 seconds, the neighbour is considered unreachable and routing tables are updated and shared with other neighbours.

Routing information to an individual node can also be requested between neighbours via UDP port 250. This use of the Transport layer by a Network layer algorithm is possible because RIP is actually an Application layer entity.

### Intra-AS Routing in the Internet: OSPF

OSPF is an LS algorithm, typically used by upper-tier ISPs, in which neighbours broadcast their routing information to all other nodes in the AS. The network manager is responsible for setting link costs (e.g., to reflect bandwidths of links) such that administrative autonomy is retained. The algorithm runs strictly in the Network layer and must implement its own equivalent to Transport mechanisms such as reliable message transfer.

OSPF was intended as a successor to RIP and has a number of advances over RIP:

* Security: exchanges can be authenticated to ensure that new routers can't simply be added to the network and mess up the routing tables. Passwords are either shared (and trasmitted in plaintext which isn't very secure) or the sending router scrambles the message, adds the MD5 hash, and the receiving router computes the MD5 of the scrambled message itself and compares it with the received MD5 to ensure that the sending router had the correct credentials.
* Multiple same-cost paths allow packets to take a different route if that route is joint shortest path
* Unicast and multicast support is included (see below)

One key advantage of OSPF is that an autonomous system can be subdivided again into *areas* for hierarchical operation. Each area consists of one *area border* router and a number of *internal routers*. Every area border router is also a member of the *backbone area* through which all messages pass. The backbone area also contains zero or more *backbone routers* that route messages between area border routers and from area border routers to the *boundary router* that connects the AS to other ASs.

Link costs determine the path taken from one node to another in the routing table. It is possible, however, that certain paths are preferred over others (for an unlimited number of reasons) and so the network manager can set the weights in order to achieve the desired routing, in a reversal of cause and effect (the routes determine the link costs).

### Inter-AS Routing: BGP

The *Border Gateway Protocol v4* (*BGP4* or simply *BGP*) is the *de facto* standard for routing between ASs. It gives each AS the means to:

* Obtain subnet reachability information from neighbouring ASs
* Propagate the reachability information to all routers internal to the AS
* Determine "good" routes to subnets based on the reachability information and on AS policy.

Crucially, BGP is the mechanism that enables subnets to advertise their own existence so that other subnets can find and route traffic to them.

BGP is complex and you'd need a lot of experience to understand it fully, but here we present a brief overview of its operation.

Pairs of connected routers share a semipermanent TCP connection (port 179) across which they send BGP messages, collectively known as a *BGP session*. Routers that connect two ASs employ *external BGP (eBGP)* sessions; routers within the same AS employ *internal BGP (iBGP)* sessions. Any two routers sharing a BGP session are called *BGP peers*.

BGP advertises the reachability of subnets by their CIDR prefix, and can aggregate subnets that have N prefix bits in common (with prefix/N). Gateway routers can advertise reachability information to other routers internal to the AS and also to gateway routers in other ASs (which will then advertise the reachability to its internal routers).

Each AS (or at least most of them) comes with an *Autonomous System Number (ASN)* that is assigned by ICANN. When BGP advertises a prefix across a BGP session, it attached *BGP attributes* that turn the advertisement into what is termed a *route*. 

*AS-PATH* (autonomous system path) is an attribute that contains the sequence of ASNs traversed in order to reach the current AS, which will append its ASN to the path before advertising it to another AS. This enables an AS to detect when a route has looped back to itself and reject the advertisement.

*NEXT-HOP* bridges the inter-AS and intra-AS routing algorithms, pointing the intra-AS algorithm to the gateway router that directs traffic to a given subnet (advertised by the inter-AS algorithm).

These two attributes are also used to choose between two alternative routes to the same subnet.

When a router receives an advertisement, it uses an *import policy* to decide whether to accept or filter the route (e.g., in cases where it does not want to send traffic through a specific AS or knows of a better route to the subnet already).

When choosing between several possible routes to a subnet, BGP will (in order):

* Eliminate all routes but those with the highest preference value (a BGP attribute optionally added by a router)
* Eliminate all routes but those with the shortest AS-PATH
* Eliminate all routes but those with the closest NEXT-HOP router
* Use BGP identifiers to choose a route from the set that remains

Routing policies can also be used to enforce specific behaviours. For example, an AS that should not forward traffic between neighbouring ASs but strictly produces or consumes traffic is known as a *stub network* and is prevented from forwarding packets by advertising no routes to other ASs, even if there are physical connections in place.

Similar policies can determine which backbone networks are permitted to carry traffic destined for other backbone networks (e.g., between networks belonging to different ISPs). These policies are usually agreed in private between ISPs to ensure that nobody is given a "free ride."

## Broadcast and Multicast Routing

Sometimes it is desirable to send a packet to *all* other nodes in the network (*broadcast routing*) or just to *some* other nodes in the network (*multicast routing*) rather than one-to-one communication (*unicast routing*).

### Broadcast Routing Algorithms

The simplest approach to reaching all other nodes would be for the source to send the same packet N times, each with a different destination address (an *N-way unicast*). This is obviously inefficient, though efficiency can be achieved if the duplication occurs further along the route by other routers. Moreover, knowing the addresses of every destination adds complexity to the algorithm. Finally, broadcasting is used in the LS algorithm to determine routes to every destination yet the N-way unicast assumes that the routes are already known (a form of circular dependency).

*Flooding* is one approach to resolving the inefficiency argument whereby the source sends the broadcast packet only to its immediate neighbours. Each neighbour then sends the packet to each of its immediate neighbours (except the originator) and so on. This, however, falls down when there are loops in the network such that packets can be forwarded indefinitely or, worse, continue being duplicated without being consumed until the traffic grinds to a halt (a *broadcast storm*).

*Controlled flooding* applies rules at every node to decide whether to forward (flood) the packet or drop it. One implementation (*sequence-number-controlled flooding*) adds the source's address plus a *broadcast sequence number* to the packet, and routers drop any packets they have already flooded. Another implementation (*reverse path flooding*) will only flood a packet that is received on a link that is on the shortest path back to the source, knowing that a packet received on any other link will be a duplicate of the one that has been (or will be) received along the shortest path.

A third implementation computes the *minimum spanning tree (MST)* of the network that leaves no node unconnected but also contains no loops, at minimal cost. This avoids the problems that come with loops (redundant packets and broadcast storms) and is efficient in that the MST need be computed only once. One algorithm for computing the MST is the *centre-based approach* whereby one node is selected as the centre (or root) of the tree and other nodes send *tree-join* messages to it, grafting each path-to-root to the tree.

In practice, sequence-number-controlled flooding is a popular choice, often augmented with a time-to-live (TTL) or equivalent field for *limited-scope flooding*.

### Multicast

Unicast requires the IP address of the sole recipient (easy); broadcast requires no IP address at all (also easy); *multicast* requires a packet to go to many but not all destinations (not so easy). Using a list of IP addresses is both inefficient and also assumes that the sender has the IP address of every recipient (which it may not).

The answer is to use *address indirection* where a single IP address is used for every member of the *multicast group* (i.e, the group has an IP address of its own). This group indirection is managed by the *Internet Group Management Protocol* and *multicast routing protocols*.

IGMP (v3) covers communication between an edge (first-hop) router and the hosts in its LAN. Packets destined for the group have an IP protocol number of 2, and the router periodically sends out a *membership_query* messages to all connected hosts, to which each host responds with a *membership_report* listing the groups to which it belongs (or to which it wants to belong). A *leave_group* message may be sent by a host to terminate membership of a group, or it may simply omit the group from its *membership_report* response (so called *soft state* management).

The multicast routing protocol then ensures that messages are delivered from a source in the group to every router connected to group members (and not to routers with no connected group members). This is commonly achieved again with a spanning tree where only group-connected routers (or intermediate routers necessary to complete the tree) are included. The tree may be either centre-based (a single tree) or source-based (one tree per source), the latter of which may be *pruned* to remove routers that are not connected to any group members by sending "prune" messages upstream.

In practice, Network layer multicasting is used but not very much; Application layer multicasting (e.g., peer-to-peer networking) is preferred at the time of publication.

# The Link Layer and Local Area Networks

