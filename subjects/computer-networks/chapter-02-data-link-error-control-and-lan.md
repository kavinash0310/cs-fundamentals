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


## CRC and sliding-window worked reasoning

CRC sender appends r zero bits to message polynomial M(x), divides by generator G(x), and appends the r-bit remainder. Receiver divides the received codeword by G; zero remainder means no detected error, not proof that no error occurred.

For stop-and-wait, if round-trip propagation dominates, utilization is roughly transmission/(RTT+transmission). With window W, utilization can approach min(1, W×transmission/(RTT+transmission)), assuming sender always has data and no loss.

Sequence numbers wrap modulo 2^k. Selective Repeat needs a sequence space at least twice the send-window size to avoid ambiguity between old and new frames. ACK/retransmission timers and duplicate handling define correctness.

**Practice:** Why doesn't parity detect two flipped bits? Even parity remains unchanged. Compare cumulative ACK in Go-Back-N with individual ACKs in Selective Repeat. Explain why checksum/CRC is not authentication; attackers can alter data and recompute them.

## Stop-and-wait versus sliding-window worked reasoning

Let frame transmission time Tt=1 ms and RTT=39 ms; ignore ACK transmission. Stop-and-wait utilization ≈Tt/(Tt+RTT)=1/40=2.5%. To keep a 10 Mbps path with RTT 40 ms busy, bandwidth-delay product is 400,000 bits, or 50,000 bytes in flight.

A sliding window of W frames can keep the line busy if W×frame-size is at least the bandwidth-delay product (modulo ACK, headers, losses, and protocol constraints). Window size and sequence-number space must be consistent.

Error detection says a frame is corrupted; ARQ adds acknowledgments/timeouts/retransmission. Forward error correction adds enough redundancy to correct some errors without retransmit. Choose based on channel and latency requirements.

**Practice:** Calculate minimum window for 1000-byte frames on 2 Mbps, RTT 20 ms: BDP=40,000 bits=5,000 bytes, so about 5 frames, ignoring overhead.