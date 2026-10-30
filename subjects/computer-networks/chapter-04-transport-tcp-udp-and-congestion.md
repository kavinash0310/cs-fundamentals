# Chapter 4 — UDP, TCP, Flow Control, and Congestion

## 1. Ports and sockets
Transport layer multiplexes applications using port numbers. A socket endpoint includes IP address and port; a connection is identified by endpoint pair and protocol.

UDP is connectionless datagrams with ports and checksum; it does not guarantee delivery/order or congestion control. TCP provides reliable ordered byte stream, connection setup, retransmission, flow and congestion control.

## 2. TCP reliability
Sequence numbers count bytes. ACKs identify next expected byte. Retransmission timers and duplicate ACK behavior detect loss. Sliding window permits in-flight bytes. Three-way handshake synchronizes initial sequence numbers; connection close uses FIN/ACK exchanges.

TCP is a byte stream, not message-preserving. Application framing must define message boundaries.

## 3. Flow versus congestion control
Flow control protects receiver buffer: advertised receive window limits outstanding data. Congestion control protects network: congestion window changes based on feedback/loss. Effective send window is constrained by both.

Slow start increases congestion window rapidly; congestion avoidance grows more cautiously. Exact algorithms vary; do not equate congestion window with receiver window.

## 4. Questions and practice
**Q1.** Which protocol preserves message boundaries? **A:** UDP datagrams; TCP is a byte stream.
**Q2.** What limits sender by receiver capacity? **A:** Receive window/flow control.
**Q3.** What protects the network? **A:** Congestion control.
Practice: trace handshake; calculate sequence/ACK numbers; explain why a lost segment may delay later bytes despite their arrival.

## Notebook work
Draw TCP state exchange and distinguish advertised window from congestion window.


## TCP sequence/ACK example

If a TCP segment starts at sequence 1000 and carries 500 data bytes, next expected byte is 1500; ACK=1500 acknowledges through byte 1499. ACKs are cumulative. SYN and FIN each consume one sequence number.

TCP receiver advertises window rwnd; sender's congestion window cwnd reflects network congestion. In-flight data is limited by min(rwnd,cwnd). Retransmission timer handles timeout; duplicate ACKs can indicate a gap and trigger fast retransmit.

Flow control protects receiver; congestion control protects the path. Slow start grows cwnd rapidly at first; congestion avoidance grows more slowly; loss response reduces sending rate. Exact modern TCP behavior varies by algorithm.

UDP has no handshake or retransmission guarantee; applications may add reliability, ordering, or congestion control themselves.

**Practice:** If next expected sequence is 4000 and a 300-byte segment begins at 4000, ACK after receipt is 4300. Explain why TCP can deliver bytes in order even if IP packets arrive out of order.

## TCP state and backpressure

Three-way handshake: client SYN, server SYN-ACK, client ACK. It synchronizes sequence state and confirms both directions can communicate. It is not user authentication.

The receiver window is advertised based on available receive buffer. If application reads slowly, advertised window may shrink to zero; sender pauses (with probes) rather than overflowing receiver. Congestion window is sender's estimate of safe network load. Both constrain in-flight bytes.

Head-of-line blocking occurs in ordered TCP stream: a missing earlier segment can delay delivery of later bytes to the application even if they arrived. Applications requiring independent streams may use separate connections or a transport with stream multiplexing.

**Practice:** A segment beginning at seq=9000 carries 700 bytes; next cumulative ACK=9700. Explain what happens if bytes 9700–9999 arrive before 9000–9699; distinguish receive-window closure from congestion response.

## Extended lecture: sequence numbers, ACKs, and congestion

TCP provides a reliable ordered byte stream, not message boundaries. Sequence numbers count bytes. An ACK number usually says “the next byte I expect,” so a cumulative ACK of 5001 confirms bytes through 5000.

**Handshake trace.** Client chooses initial sequence x and sends SYN(seq=x). Server replies SYN+ACK(seq=y, ack=x+1). Client sends ACK(seq=x+1, ack=y+1). SYN consumes one sequence number despite carrying no application byte. FIN also consumes one. TCP connections are identified by the endpoint pair (source IP, source port, destination IP, destination port), allowing many connections between the same hosts.

If a sender transmits bytes 1000–1499, the payload length is 500. An ACK of 1500 means the receiver expects byte 1500 next. If later data arrives out of order, TCP may buffer it and repeat a cumulative ACK for the missing byte; selective acknowledgments can report additional received ranges.

### Flow control and congestion control differ

The receiver advertises a receive window rwnd to prevent its buffer from overflowing. The sender's congestion window cwnd limits load placed on the network. Outstanding data is constrained by approximately min(rwnd,cwnd). Flow control protects the receiver; congestion control protects the path.

Classic congestion avoidance increases cwnd roughly additively per RTT and reduces it multiplicatively after congestion (AIMD). Slow start initially grows cwnd exponentially per RTT until a threshold or loss event. Modern TCP variants differ in details, but the principle is to infer network capacity from feedback rather than send without limit.

~~~text
cwnd
 ^       / steep (slow start)
 |      /----
 |     /      / linear (avoidance)
 |____/______/____________> time
       loss: reduce window, recover
~~~

UDP preserves datagram boundaries and has low protocol overhead, but does not guarantee delivery, order, or congestion control at the transport layer. Applications using UDP still need appropriate congestion behavior when sending over the public Internet.

### Solved checks

1. A TCP segment begins at sequence 700 and carries 300 bytes. Next expected byte is 1000.
2. Does a TCP ACK of 1000 mean byte 1000 was received? No; it means 1000 is the next byte expected.
3. Which window protects the receiver's memory? rwnd.
4. Which window reflects sender's estimate of safe network load? cwnd.
