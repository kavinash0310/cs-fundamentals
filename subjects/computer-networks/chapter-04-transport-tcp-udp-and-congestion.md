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
