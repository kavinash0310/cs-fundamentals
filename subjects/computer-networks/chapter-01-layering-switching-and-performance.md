# Chapter 1 — Layering, Switching, and Network Performance

## 1. Why layers
Layering divides communication into services. Each layer uses the layer below and offers a service above. Encapsulation adds headers as data moves down; receiver removes them in reverse.

~~~text
application data
transport [TCP header | data]
network   [IP header | segment]
link      [frame header | packet | trailer]
physical  bits/signals
~~~

OSI has seven conceptual layers; Internet architecture commonly groups link, internet, transport, application. Models help reasoning; real protocols do not always fit cleanly.

## 2. Switching
Circuit switching reserves a path/resources. Packet switching shares links statistically; datagrams route independently, while virtual-circuit networks set up a logical path. Store-and-forward switches receive a packet before forwarding.

## 3. Delay and throughput
Total delay includes processing, queueing, transmission, propagation. Transmission delay = packet bits / link rate. Propagation delay = distance / signal speed. Bandwidth is capacity; throughput is achieved rate; goodput excludes overhead/retransmissions.

Example: 12,000-bit packet on 3 Mbps link takes 4 ms to transmit. If path is 600 km and propagation speed 2×10^8 m/s, propagation is 3 ms.

Bandwidth-delay product = bandwidth × RTT; it estimates bits that can be in flight.

## 4. Questions and practice
**Q1.** Which delay is bits/rate? **A:** Transmission.
**Q2.** What is encapsulation? **A:** Adding layer headers around payload.
**Q3.** Bandwidth-delay product with 10 Mbps and 50 ms RTT? **A:** 500,000 bits.
Practice: compute delay components; trace encapsulation; compare circuit and packet switching.

## Notebook work
Draw sender/receiver layers and label headers at each hop.


## End-to-end timing example

A 1500-byte packet on a 10 Mbps link has transmission delay 1500×8/10,000,000=1.2 ms. If propagation is 5 ms, one-link delay (ignoring processing/queueing) is 6.2 ms. On a store-and-forward path with three such links, one packet has about 3×(1.2+5)=18.6 ms; pipelining multiple packets changes total message completion time.

For packet length L bits and rate R, transmission=L/R. Propagation=d/s and is independent of packet length. Queueing varies with load and can dominate near saturation.

For stop-and-wait over a path with RTT=40 ms and transmission time=2 ms, utilization is approximately 2/(40+2)=4.76% if ACK time is negligible. Sliding windows keep more data in flight; bandwidth-delay product estimates the needed window.

**Practice:** Calculate packet transmission time for 8000 bits at 4 Mbps =2 ms; distinguish RTT from one-way delay; explain why higher bandwidth does not reduce propagation time.

## Socket API and client-server flow

TCP server sequence in a typical sockets API:
~~~text
socket -> bind(local address) -> listen -> accept
client: socket -> connect(server)
server/client: send/receive bytes -> close
~~~
UDP often uses socket then sendto/recvfrom without a connection handshake. Exact calls differ by operating system and language.

A port identifies an application endpoint on a host. A TCP connection is identified by local/remote address and port plus protocol. Server may accept many connections and must handle partial reads/writes; TCP gives a byte stream, not one message per send call.

**Layering check:** DNS resolves a name; IP forwards packets; TCP provides reliable ordered bytes; HTTP defines request/response semantics. One browser request uses multiple protocols and often several connections/streams.

**Practice:** Explain why a server must loop until a complete application message is read; compare connect() in TCP with datagram sendto().

## Extended lecture: quantify end-to-end performance

For a path with links and routers, distinguish four delays at each hop: processing (inspect header), queueing (wait for service), transmission (push bits onto a link), and propagation (signal travel). For a packet of L bits on a link of rate R bit/s, transmission delay is L/R. For distance d and propagation speed v, propagation delay is d/v. Increasing link rate reduces serialization time but does not make the signal travel faster.

**Worked example.** A 1,200-byte packet crosses a 10-Mbit/s link with 20 ms one-way propagation. L=9,600 bits, so transmission is 9,600/10,000,000=0.00096 s=0.96 ms. Ignoring processing and queues, one-hop delay is 20.96 ms. For store-and-forward across three equal links, a packet must be received before retransmission at each router; the packet's serialization contributes three times, as does propagation if all links span equal distance. Pipelining lets later packets enter while earlier ones are forwarded, so steady-state throughput can be much higher than the first-packet delay suggests.

### Bandwidth-delay product

BDP = bottleneck bandwidth × round-trip time. At 100 Mbit/s and 40 ms RTT, BDP=4,000,000 bits=500,000 bytes. Roughly half a megabyte must be in flight to keep that path fully utilized, absent losses and other limits. This explains why a tiny sender window can cap throughput even on a fast link.

~~~text
sender -- packet in flight ----------------------> receiver
       <---------------------- ACK (RTT later)
window too small: sender waits idle between bursts
~~~

Throughput is bounded by the minimum of link capacity, sender/receiver limits, and window/RTT. Queueing delay rises when offered load approaches service capacity; buffering may absorb bursts but cannot create bandwidth. A router dropping packets at a full queue is a signal that the offered load exceeded what it could serve.

### Circuit and packet switching

Circuit switching reserves a path's resources for a session; it can offer predictable service but may waste reserved capacity during silence. Packet switching statistically multiplexes flows; it uses links efficiently for bursty traffic but introduces queueing and possible loss. In a datagram network, each packet can be routed independently; a virtual circuit establishes logical path state first.

### Practice with solutions

1. A 2,000-byte frame on 20 Mbit/s: transmission time =16,000/20,000,000=0.8 ms.
2. RTT=50 ms, capacity=8 Mbit/s: BDP=400,000 bits=50,000 bytes.
3. Does higher bandwidth reduce propagation delay? No; propagation depends primarily on distance and medium.
4. Why can throughput be low with no packet loss? A small window, high RTT, receiver limit, or queueing can constrain it.
