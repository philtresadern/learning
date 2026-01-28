# Pattern-Oriented Software Architecture

## Patterns
A pattern has three branches:

- Context: Design situation giving rise to a design problem
- Problem: Set of forces repeatedly arising in the context
- Solution: Configuration to balance the forces; structure with components and relationships; run-time behaviour.

[This reminds me of the Given-When-Then approach to writing tests; 
Given (the context), When (the problem arises) Then (apply the solution).]

## Architectural Patterns

### From Mud to Structure

#### Layers

[TBC]

#### Pipes and Filters
In a Pipes and Filters architecture, data flows from a Source (S) to a Sink (T) via Filters (F1, F2, ...) connected by Pipes.

The example given here is of a compiler that takes Source code, parses it in a number of stages (each represented by a Filter)
and finally outputs binary code in one of many formats each with its own corresponding backend. 

Each filter transforms, refines or enriches its input data before outputting the results to the next filter.

A passive Filter receives data from an adjacent active Filter that executes a *push* action,
or provides data to an adjacent active Filters when it executes a *pull* action.

Any or all filters can be active and therefore trigger a processing cycle. A cycle typically starts when a *read* request
cascades through the pipeline toward the Source which provides data to the first Filter. The processed data then cascades
in the opposite direction until it reaches the Sink.

One common implementation uses a Pipe between Filters to buffer data such that the pipeline can be parallelised for efficiency.
In this case, F1 pushes data to the buffering Pipe and is then free to pull more data from the Source and begin processing it.
Meanwhile, F2 pulls data from the buffering Pipe when it is ready, processes it and pushes it to the Sink, making it free
to pull more data as soon as it is available. F1 and F2 can process their own respective data simultaneously.

Each Filter in the pipeline depends only on the output of the preceding Filter, so you must segment the process carefully,
especially if you plan to reimplement Filters or make them interchangeable at any point.

You must define the data format to be passed along. If data conversion (e.g., binary to ASCII) is required at any point,
this can be implemented as a Filter in its own right and inserted into the pipeline. Because Filters process a stream of data,
it is important to have some token that signifies the end of the stream.

Filters can be connected directly such that one Filter pulls from or pushes to its immediate neighbour via a function call.
However, this couples Filters in a way that makes it harder to redesign the pipeline in the future.
It is therefore better to have Filters push to and pull from an intermediate Pipe (e.g., a message queue or a UNIX pipe)
inserted between Filters (as an abstraction layer?) so that Filter implementations can be swapped in and out easily.

Consider the cost of switching context and transferring data when designing the pipeline as both can incur considerable
overhead.

Flexibility in the Filter's use (e.g., command line arguments for a CLI Filter) makes reuse easier,
as does a global environment or reporsitory that all Filters can access. Filters should, however, be easy to use
which excessive flexibility can impair.

Error handling is complex, especially when Filters operate in parallel (e.g., on different processes or threads) and many
may write to an error stream (e.g., stderr) at the same time. Often, the only course of action on discovering an error
in the input is to restart the pipeline from the beginning, though some implementations process data in batches and 
insert marker values between batches so that processing can restart from a known good "checkpoint."

Pipes can also include a feature to write intermediate data to file as it passes through the Pipe such that it can be 
retrieved and examined after the pipeline has completed.

Benefits:
* Pipelines do not enforce the creation of intermediate files (which clutters storage) but allows them if needed.
* Filters with simple interfaces are easy to swap in and out of a pipeline, leading to flexible design and rapid prototyping.
* A Pipe and Filters architecture can act as a Composite design pattern in that the pipeline can itself be treated as a Filter in another, larger pipeline.
* Where many Filters are active, the pipeline can be run in parallel for efficiency.

Liabilities:
* Sharing state information is expensive, especially when many Filters need access to the same data.
* When Filters are small and simple, the time spent transferring data between them can exceed the time spent processing the data.
* Some operations cannot be implemented incrementally and requires the whole stream before processing can even start (e.g., sorting).
* Keeping Filters synchronized in parallel can involve lots of starting and stopping, and idle time in between.
* Transforming data between formats adds further overhead.
* Error handling is complex in the P&F architecture, and mission-critical applications are often better implemented with [Layers](#layers).

#### Blackboard

The Blackboard pattern is (or at least was) particularly suited to immature domains where a sequential solution is not obvious, or where different data representations may be required to solve partial problems.

The example given is of speech recognition where the raw input format is a waveform that is then parsed into phones and finally assembled into words and phrases.

The *forces* involved include uncertain or partial solutions that may not be applied in a sequential order. In fact, the absence of a sequential order may allow partial solutions to be computed in parallel, which is a property that should be exploited.

[I wonder how this pattern resembles Microservices in the current climate.]

The *solution* behind this architecture is one of many programs working independently on the same data, under the supervision of a central control component (much as people working together on a problem written on a blackboard will take turns adding their contributions, not necessarily in a strict order). The pattern lends itself to trying out different partial solutions and algorithms.

Items of data - either partial or complete - are known as Knowledge Sources and stored on the Blackboard. Each has a condition part, that determines whether the Source can make a contribution, and an action part that effects a change in the state of the Blackboard. 

[I confess, this Pattern sounds to me a little old-fashioned, and I'm going to stop at *Implementation* and skip to the next section.]

### Distributed Systems

#### Broker

The Broker pattern - as used in Microsoft's Object Linking and Embedding (OLE) and OMG's Common Object Request Broker Architecture (CORBA) - is used for distributed (possibly heterogenous) computing across multiple processors in a machine or nodes in a network. ("The network is the computer.")

The *forces* involved include being able to access services through remote, transparent invocations; being able to change components at run-time; and components being unaware of (and uninterested in) the implementation details and location of their collaborators.

The *solution* is to introduce a Broker component to which both clients and servers connect, rather than trying to connect to each other. A Client sends a request to the Broker which decides what Server to forward the request to; the Server sends its response to the Broker which forwards it back to the originating Client. Clients and Servers (objects) can live on separate and heterogenous nodes in a distributed network without even being aware of it.

The *structure* comes in six parts:

1. The *server* - which can be a service or an object (in the OOP sense), and can be general or specific to an application domain - receives a request (e.g., a HTTP request), constructs the HTML, runs any server-side scripts (e.g., CGI), and returns the HTML response along with any client-side scripts (e.g., Java applets).
2. The *client* is an application that issues requests to the *Broker* via a *client-side proxy* (see below). Note that clients needn't know where servers are located or even what servers are available (such that adding new servers at run-time is possible). Moreover, a server that issues a request to a second server is technically a client of the second server. (Client and Server are therefore not set in stone.)
3. The *broker* is the conduit for communication between client and server, via an API. This API will include services for servers to register themselves with the broker. If a server is registered with a different broker, the first broker must forward requests and receive responses from the second broker, such that brokers must be able to interoperate. In practice both the client and server have an Internet Gateway which, along with the Internet itself, act as the Broker between client and server on a wide area network.
4. A *client-side proxy* sits between the Client and the Broker and controls the message delivery between the two by packaging request arguments and unpacking response arguments.
5. Similarly, the *server-side proxy* sits between the Broker and the Server, controlling communication between them by unpacking requests and packaging responses.
6. A *bridge* effectively sits between two brokers (possibly on separate nodes in the network) and acts as an exchange such that messages can be sent and received between brokers. A bridge is not always needed.

There are three distinct *dynamics* of a Broker pattern:

1. The broker starts its event loop; the server initializes itself and sends a *register* request to the broker; the broker stores the server's details in a repository and acknowledges the registration; the server then enters its event loop and awaits a request.
2. The client makes a request, which is packaged by the client-side proxy and sent to the Broker, which looks up the destination server and sends the request message to the server-side proxy which unpacks the arguments of the request (e.g., the method to call) and calls the method on the server. Then the response goes back by following that sequence in reverse.
3. As above, only there are two Brokers, each with its own Bridge. Broker A receives the request message from the client-side proxy, sends it to its Bridge (A) that converts the message from format A to a common network format, and forwards it to Bridge B. Bridge B converts the common-format message to format B and sends it to Broker B for forwarding to the server-side proxy.

The *implementation* of the Broker pattern (as described here) is complex and often hinges on whether you use a binary standard or an interface definition language (IDL). [Much of this discussion went over my head.]

One part I think I did understand is the role of proxies. The client-side proxy lives in the same process as the client and acts *as if it were the server that is located in the same process* (i.e., the two should be indistinguishable). Therefore, a client-side proxy should have the same API as the server and return the same data that the server would have done if it were in the same process. The proxy, however, receives the method call from the client, packages up the data and sends it on to the Broker for transmission to a remote server that lives in an entirely different process. The server-side proxy does the same with respect to the client and should therefore implement the client's API.

Implementing the Broker itself requires defining the protocol between proxy and broker, maintaining a routing map between servers, giving names to servers when they register themselves with a broker, (optionally) supporting dynamic method invocation, and error handling. If an IDL is used, there needs to be a compiler for every language (better still, the IDL should be implemented as a framework to which developers can add their own desired compiler.)

Variants of the Broker pattern include:

* Direct Communication Broker System: clients communicate directly with the server once the broker tells the client what protocol to use.
* Message Passing Broker System: [this sounds as though] the server provides only one function in its API, and that function processes data differently depending on the *type* of message which is contained in the message data itself. [This is marked as being in contrast to Brokers, typically built using message-passing interfaces, that offer Remote Procedure Call (RPC) interfaces. I don't understand this.]
* Trader System: the client calls a *service* rather than a *server* such that the request can go to one of many servers that provide the requested service.
* Adapter Broker System: uses more than one *adapter* to support multiple strategies, adding another abstraction layer to the implementation to increase flexibility.
* Callback Broker System: less of a distinction between clients and servers - both generate *events* and callbacks are defined that react to events of a particular type. The "response" is then just another event to which the "client" will react.

These variants are not mutually exclusive and can be combined (e.g., Direct Communication and Trader).

Examples of Broker implementations include CORBA, Microsoft OLE and the humble web browser.

*Benefits* of the Broker pattern include:

* Location transparency: clients need not know where servers are located (and vice versa)
* Changeability and extensibility: as long as interfaces stay the same, you can swap implementations of all of these components without changing client code.
* Portability: clients and servers need not know what operating system they are using; only the Broker need change, and even then by separating its implementation into layers (see above) only the lowest layers need change.
* Interoperability between Brokers: using Bridges between Brokers to translate from the Broker protocol to a common protocol means that Brokers can effectively talk to each other, regardless of their own internal workings.
* Reusability: The levels of abstraction here mean that Brokers are inherently easy to reuse across applications.

The corresponding *liabilities* include:

* Restricted efficiency: the layers of indirection in the Broker pattern can make it seem slow compared to direct communication.
* Lower fault tolerance: there are more potential points of failure in this long chain of links.

Clients are easier to test since much of the transmission logic resides in the Broker. The Broker, however, is a pain to test because of the many possible points of failure: not only could the server enter an error state but any point along the transmission path could, too.

See also [[Forwarder-Receiver]], [[Proxy]], [[Client-Dispatcher-Server]] and Mediator (which looks like a Broker but operates on a single process in a single machine, sometimes responding to requests itself.)

### Interactive Systems

#### Model-View-Controller

#### Presentation-Abstraction-Control

### Adaptable Systems

#### Microkernel

#### Reflection

## Design Patterns

### Structural Decomposition

#### Whole-Part

### Organization of Work

#### Master-Slave

### Access Control

#### Proxy

### Management

#### Command Processor

#### View Handler

### Communication

#### Forwarder-Receiver

#### Client-Dispatcher-Server

#### Publisher-Subscriber

## Idioms

## Pattern Systems

## Patterns and Software Architecture

## The Pattern Community

## Where Will Patterns Go?

[TBC]

