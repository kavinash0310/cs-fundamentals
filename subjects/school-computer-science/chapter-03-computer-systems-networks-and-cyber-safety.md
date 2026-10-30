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

## Extended lesson: follow data through a computer and network

A keyboard event becomes input handled by software; the CPU executes instructions using registers and main memory; persistent storage keeps data after power-off; output reaches a screen or printer. RAM is fast working memory and volatile. SSD/HDD storage is persistent. The CPU's control unit directs operations, while the ALU performs arithmetic and logic.

On a network, data is divided into packets. A switch forwards frames within a local network using link-layer addresses; a router forwards packets between networks using IP addresses. DNS finds an address associated with a domain name. HTTPS uses TLS to protect data in transit and authenticate a server under its certificate trust model.

### Safety scenario: suspicious account message

A student receives “Your school account will close today—open this link and sign in.” Safe response:
1. Do not open the link or enter credentials.
2. Check the sender and verify through a known school channel.
3. Report the message to the teacher/administrator.
4. If credentials were entered, change them through the official site and notify the administrator.
5. Use unique passwords and multifactor authentication where available.

A password manager can create and store unique passwords. Public Wi-Fi is not a reason to ignore certificate warnings; HTTPS protects traffic only when the browser validates the connection. Never install unknown “security updates” from pop-up links.

### Data, privacy, and responsible use

Personal data can identify or locate a person. Collect only what a task needs, share only with authorized people, and check privacy settings before posting. Backups reduce data-loss risk but should be protected too. Copyright and attribution matter when copying images, code, or text. Digital citizenship includes respectful communication and reporting harmful behavior through safe channels.

### Questions

1. Which component loses its contents when power is removed: RAM or SSD? RAM.
2. What does DNS help resolve? A domain name to records such as IP addresses.
3. Does HTTPS prove a website's claims are truthful? No; it protects a connection and authenticates a domain certificate, not the truthfulness of content.
4. What should a learner do with a suspected phishing message? Avoid interaction, verify via trusted channel, and report it.
