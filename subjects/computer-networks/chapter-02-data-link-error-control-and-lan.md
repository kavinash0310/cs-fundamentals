# Chapter 2 — Data Link, Error Control, and LANs

## 1. Framing and error detection
Data-link layer delivers frames over one link. Framing marks boundaries. Error detection adds redundancy:
- Parity detects odd number of bit flips.
- Checksum sums words and complements result.
- CRC treats bits as a polynomial and divides by generator polynomial; nonzero remainder signals corruption.

CRC detects many burst errors but is not encryption and does not correct arbitrary errors.

## 2. Reliable transfer and flow
Stop-and-wait sends one frame then waits for ACK; simple but wastes high-delay links. Sliding window allows multiple outstanding frames. Sequence numbers distinguish retransmissions. Go-Back-N retransmits from a missing frame; Selective Repeat retransmits only missing/corrupt frames.

Flow control prevents a fast sender overwhelming a receiver. Error control detects/retransmits loss or corruption.

## 3. Medium access and Ethernet
Shared media need MAC access. CSMA/CD historically detects collisions in half-duplex Ethernet; modern switched full-duplex Ethernet avoids collisions on point-to-point links. Ethernet frames carry source/destination MAC, type/length, payload, and frame check sequence.

Wi-Fi uses CSMA/CA because a wireless station cannot reliably detect collisions while transmitting. It may use ACKs and optional RTS/CTS to reduce hidden-terminal collisions.

## 4. Questions and practice
**Q1.** CRC provides confidentiality? **A:** No, only error detection.
**Q2.** Which sliding window retransmits only selected losses? **A:** Selective Repeat.
**Q3.** Why collision avoidance in Wi-Fi? **A:** Detecting a collision during transmission is difficult.
Practice: calculate stop-and-wait utilization; compare Go-Back-N/Selective Repeat; show parity on a byte.

## Notebook work
Draw a sliding-window timeline with one lost frame and ACK sequence.
