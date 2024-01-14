# Interconnection Networks

## Concepts
Today we talk about "a-network-on-chips"

Terminology
- Network node: a network endpoint connected to a router/switch(e.g. processor caches, the memory controller)
- Network interface: connects nodes to the network
- Switch/router: connects a fixed number of input links to a fixed number of output links
- Link: a bundle of wires carrying a signal

Design issues
- Topology: how switches are connected via links
- Routing: how a message gets from its source to its destination in the 
network(static or adaptive)
- Buffering and flow control: how does the network manage buffer space?

Properties of an interconnect topology
- Bisection bandwidth: 
  -  Common metric of performance for recursive topologies
  -  Cut network in half, sum bandwidth of all severed links
  -  Warning: can be misleading as it does not account for switch and routing efficiencies

- Blocking vs. non-blocking: 
  - If connecting any pairing of nodes is possible, network is non-blocking (otherwise, 
it’s blocking)

## Interconnect topologies


