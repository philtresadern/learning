# Pattern-Oriented Software Architecture

## Patterns
A pattern has three branches:

- Context: Design situation giving rise to a design problem
- Problem: Set of forces repeatedly arising in the context
- Solution: Configuration to balance the forces; structure with components and relationships; run-time behaviour.

[This reminds me of the Given-When-Then approach to writing tests; 
Given (the context), When (the problem arises) Then (apply the solution).]

## Architectural Patterns

### Layers

### Pipes and Filters
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

### Blackboard

[TBC]

