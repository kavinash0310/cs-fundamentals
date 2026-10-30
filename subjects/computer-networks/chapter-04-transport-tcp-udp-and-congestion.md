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