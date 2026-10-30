# Chapter 3 — IP, Subnets, Fragmentation, and Routing

## 1. IPv4 and CIDR
IPv4 address has 32 bits. CIDR prefix /p marks network bits; host bits count is 32-p. A /p block contains 2^(32-p) addresses before reserved/network conventions. Subnet mask has p leading ones.

Example 192.168.10.0/24 has 256 addresses; splitting into /26 gives four blocks of 64 addresses: .0, .64, .128, .192. Network and broadcast addresses are traditionally reserved in IPv4 subnets.

NAT rewrites addresses/ports between private and public space; it conserves public addresses but complicates end-to-end reachability.

## 2. Datagram forwarding and fragmentation
Routers forward based on destination prefix using longest-prefix match. TTL/hop limit prevents indefinite loops. IPv4 may fragment a datagram to fit link MTU; fragments reassemble at destination. Fragment offset is measured in 8-byte units; all non-final payload lengths must be multiples of 8. IPv6 routers do not fragment packets in transit.

## 3. Routing
Distance-vector routers exchange distances with neighbors; Bellman-Ford relation dₓ(y)=minᵥ(c(x,v)+dᵥ(y)). Count-to-infinity can occur after failures; split horizon/poisoning can help.

Link-state routers flood link costs and run Dijkstra locally. It converges faster with global topology knowledge but needs more state and flooding.

Routing chooses paths; forwarding moves each packet through the local data plane.

## 4. Questions and practice
**Q1.** Host bits in /20? **A:** 12.
**Q2.** How many addresses in /27? **A:** 32.
**Q3.** Which route wins: /16 or matching /24? **A:** /24 by longest-prefix match.
**Q4.** Distance-vector algorithm? **A:** Bellman-Ford style.
Practice: split a /24 into /26s; calculate a fragment offset; compare link-state and distance-vector.

## Notebook work
Write the binary mask for /26 and trace a packet through two routers.


## Subnetting and fragmentation worked examples

For 192.168.4.0/26, block size is 64 addresses. Subnets begin .0, .64, .128, .192. First subnet's network is .0 and broadcast .63; commonly usable host range .1–.62. The mask is 255.255.255.192.

IPv4 fragmentation example: payload length 4000 bytes, MTU 1500, IPv4 header 20 bytes. Each full fragment carries at most 1480 data bytes, divisible by 8. Data chunks can be 1480,1480,1040; total packet sizes 1500,1500,1060. Offsets: 0,185,370 because offset unit is 8 bytes. MF=1 on all but last.

Longest-prefix lookup chooses the most specific matching route. Distance-vector can suffer count-to-infinity; link-state floods topology then computes shortest paths. Both require convergence after changes.

**Practice:** How many /28 subnets fit in /24? 16. Why choose /28 for a small LAN? It allocates 16 addresses (conventionally 14 usable). Compute prefix for 64 total addresses: /26.