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
