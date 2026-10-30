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

## Extended lecture: CRC and reliable delivery

A CRC treats a bit string as a polynomial over GF(2), where subtraction is XOR. If generator G has degree r, append r zero bits to the message, divide by G using XOR long division, and replace the appended zeros with the r-bit remainder. The receiver divides the received codeword by G; a nonzero remainder indicates corruption (though some error patterns can be undetected).

**Small example.** Message 1101 and generator 1011 (polynomial x³+x+1). Append three zeros: 1101000. XOR long division leaves remainder 001, so transmit 1101001. Recheck by dividing 1101001 by 1011: remainder 000. The receiver does not “correct” the error from this check; it detects likely corruption and relies on retransmission or higher-layer handling.

### ARQ protocols as state machines

Stop-and-wait sends one frame and waits for its ACK. With negligible ACK transmission time, link utilization is approximately frame transmission time / (frame transmission time + RTT). If frame serialization is 1 ms and RTT is 19 ms, utilization is about 1/20=5%, even without errors. A one-bit sequence number distinguishes a retransmitted old frame from a new one.

~~~text
send frame 0 -> wait ACK 0 -> send frame 1 -> wait ACK 1
        timeout? retransmit same sequence number
~~~

Sliding-window protocols keep several frames outstanding:
- Go-Back-N uses cumulative acknowledgments; after loss, retransmit the missing frame and later frames in the window.
- Selective Repeat individually acknowledges and buffers out-of-order frames; it saves retransmissions but needs more receiver state.
- To avoid ambiguity after sequence-number wraparound, Selective Repeat's window must be at most half the sequence-number space.

For m sequence bits, sequence space is 2^m. With m=3, Go-Back-N can use sender window up to 7, while Selective Repeat must use windows at most 4.

### LAN switching

A learning Ethernet switch records source MAC address → ingress port. Unknown destinations are flooded within the VLAN; known destinations are forwarded only to the learned port. Broadcast frames stay in the broadcast domain. VLANs partition logical LANs; communication between VLANs needs routing.

### Solved checks

- Why is a CRC not encryption? It detects accidental error patterns but provides no secrecy or keyed authenticity.
- What happens if ACK is lost after a frame arrived? Sender times out and retransmits; sequence numbers let the receiver identify a duplicate.
- With 4-bit sequence numbers, what is the maximum Selective Repeat window? 8 frames.
