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

## Routing and packet-path example

Host A on subnet X sends to a host outside X. A compares destination prefix with its own subnet; since it is remote, it sends the frame to its default gateway's MAC address, while the IP destination remains the remote host. Router removes link header, examines IP destination/TTL, selects next hop, decrements TTL, and wraps packet in a new link frame for outgoing link.

Distance-vector neighbors exchange route cost vectors; poisoned reverse can prevent some loops but not every topology issue. Link-state advertisements are flooded; each router builds topology database and runs shortest path. Routing-table updates eventually affect forwarding entries.

**Practice:** Distinguish MAC destination from IP destination at first hop; trace TTL across two routers; select longest-prefix route among /8, /16, /24 matches.

## Extended lecture: subnet, route, and fragment calculations

An IPv4 address has 32 bits. A /n prefix fixes the first n network bits; the remaining 32-n bits identify host addresses within that subnet. For a normal subnet, addresses with all host bits 0 and all host bits 1 are the network and directed-broadcast addresses, leaving 2^(32-n)-2 usable hosts. Special-purpose prefixes have exceptions.

**Worked subnet.** 192.168.10.70/26 has 6 host bits, so each block has 64 addresses. The last-octet block boundaries are 0, 64, 128, 192. Therefore network=192.168.10.64, broadcast=192.168.10.127, usable range .65–.126. To find the block quickly, floor(70/64)×64=64.

Routers use longest-prefix match: if destination 10.2.3.4 matches 10.0.0.0/8 and 10.2.0.0/16, choose /16 because it is more specific. A default route 0.0.0.0/0 is used only when no more-specific route matches.

### Fragmentation arithmetic

Suppose an IPv4 datagram has a 20-byte header, total length 4,000 bytes, and must cross an MTU of 1,500 bytes. Each non-final fragment can carry at most floor((1500-20)/8)×8 = 1,480 payload bytes because fragment offsets count 8-byte units. Original payload=3,980 bytes, so fragments carry 1,480, 1,480, and 1,020 bytes. Their total lengths are 1,500, 1,500, and 1,040. Offsets are 0, 185, and 370 (offset = payload bytes before fragment / 8). The first two have MF=1; the last has MF=0.

~~~text
original payload: [0 ............. 1479][1480 ........ 2959][2960 ... 3979]
fragment offset:          0                     185                370
~~~

Fragmentation adds headers and can cause the receiver to wait for all pieces before reassembly. IPv6 routers do not fragment packets in transit; the source adapts packet size using path-MTU discovery.

### Routing concepts

Distance-vector protocols share route distances with neighbors and use Bellman-Ford-style updates; count-to-infinity can occur after failures. Link-state protocols flood topology information and run Dijkstra's algorithm locally. Static route selection and forwarding are distinct: a routing protocol computes the table, while each packet lookup applies longest-prefix match.

### Questions with answers

1. How many usable hosts in /27? 2^5-2=30 in an ordinary IPv4 subnet.
2. Why must non-final IPv4 fragment data be a multiple of 8 bytes? The offset field uses 8-byte units.
3. Which route wins, /16 or /24, when both match? /24.
