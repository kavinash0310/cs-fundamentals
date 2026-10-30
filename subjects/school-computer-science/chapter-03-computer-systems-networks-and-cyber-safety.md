# Chapter 3 — Systems, Networks, and Cyber Safety

## 1. Computer systems
Input, processing, storage, and output describe a basic information-processing cycle. CPU executes instructions; RAM is volatile working storage; persistent storage retains data. Operating system manages programs, devices, files, and permissions.

Binary encodes data with bits. Boolean logic supports decisions and circuits. Distinguish data from its representation: text, image, audio, and number encodings use different conventions.

## 2. Networks and web
A network connects devices; router forwards packets between networks; switch connects devices in a LAN. DNS maps names to addresses; HTTP transfers web requests; HTTPS adds encrypted/authenticated transport protection. Explain a web request step by step without suggesting the browser directly “looks up a website” in one operation.

## 3. Cyber safety and responsible use
Use unique passwords and multifactor authentication, keep software updated, verify links/senders, protect personal data, back up important work, and report incidents. Discuss phishing, malware, privacy, cyberbullying, copyright, accessibility, and digital well-being at the age-appropriate level.

## 4. Questions and practice
**Q1.** RAM volatile means? **A:** Contents are lost when power is removed.
**Q2.** DNS role? **A:** Name-to-record lookup, commonly including address mapping.
**Q3.** Why HTTPS? **A:** Protects data in transit and authenticates server under certificate validation.
Practice: map a web request; classify storage; make a safe-response checklist for a suspicious message.

## Notebook work
Draw computer system components and a client-router-server network.


## Explain a website request accurately

1. Browser parses URL and may check caches/service workers.
2. Resolver obtains DNS records or uses cached answer.
3. Client establishes transport/security connection as needed.
4. It sends an HTTP request to server/CDN.
5. Server authorizes and processes; response returns with status and headers.
6. Browser parses HTML, then requests CSS/images/scripts, often in parallel.
7. Rendering/layout/painting update the screen.

Layers provide a teaching model, but real paths include proxies, caches, TLS, and multiple protocols. HTTPS protects communication in transit; it does not make a malicious website trustworthy or protect a compromised endpoint.

**Cyber-safety classroom scenario:** A learner receives a message asking for school password. Pause, inspect sender/link independently, do not enter credentials, report through school channel, and delete only according to policy.

**Practice:** Draw local LAN plus router and server; distinguish RAM and storage; name one privacy risk from oversharing personal data.