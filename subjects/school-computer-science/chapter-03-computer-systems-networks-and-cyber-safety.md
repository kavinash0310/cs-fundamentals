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

## 5. Trace a web request across layers

A learner enters a URL into a browser. The URL includes a scheme, host, optional port, path, and possibly query. The browser resolves the host through DNS, establishes a transport connection, protects HTTPS with TLS, sends an HTTP request, receives a response, and may request additional files.

~~~text
URL
 | parse scheme + host + path
 v
DNS resolver -> address
 |
 v
transport connection -> TLS for HTTPS
 |
 v
HTTP request -> server/CDN -> HTTP response
 |
 v
HTML/CSS/JavaScript/images -> browser rendering
~~~

At each stage ask what information is used: DNS needs the hostname; IP routes packets; TCP or QUIC carries transport data; TLS validates identity and protects a connection; HTTP supplies method, path, headers, and optional body. A web page may use many requests. A DNS answer can be cached; it is not necessarily looked up afresh for every click.

A switch forwards frames within a LAN. A router sends packets between IP networks. They answer different questions: “Which local device should receive this frame?” versus “Which next network hop moves this packet toward its destination?”

## 6. Hardware, operating system, and storage

The CPU repeatedly fetches, decodes, and executes instructions. Registers hold small, fast values used immediately; cache keeps recently used data closer to the CPU; RAM stores active program/data; SSD/HDD keeps information persistently. This hierarchy trades capacity, speed, and cost.

~~~text
CPU registers -> cache -> RAM -> SSD/HDD
fastest/smallest                    slowest/largest
volatile working state               persistent files
~~~

The operating system creates and schedules processes, manages memory, provides file and device services, and enforces permissions. A file name is not the file's physical location: the filesystem maps names and directories to storage blocks and metadata. A process can be denied access even if the file exists.

## 7. Cyber-safety threat model

For a suspicious message, separate the asset, threat, weakness, and action:

- Asset: account credentials, student records, device, or money.
- Threat: phishing, malware, impersonation, data theft, or harassment.
- Weakness: reused password, unexpected attachment, exposed personal details, or unpatched software.
- Control: verify through a known channel, use unique credentials and MFA, update software, restrict access, and report an incident.
- Recovery: change compromised credentials through the official site, notify the responsible administrator, and restore clean data from a protected backup if needed.

A credible classroom response does not blame a learner for reporting a mistake. If someone clicked a suspicious link, stop further interaction, report promptly, and follow school policy. Fast reporting can reduce harm.

HTTPS protects data in transit and helps authenticate the server name when certificate checks succeed. It does not prove that content is true, that the business is honest, or that a downloaded file is safe. A lock icon is not a complete security review.

### Privacy, accessibility, and responsible practice

Collect only information needed for the learning task. Use school-approved services for student data, limit access, and avoid placing identifiable information in public examples. Backups should be access-controlled and periodically checked. Give credit for reused code, images, and text; “found online” does not mean free to copy.

Cyberbullying and unsafe content require a trusted reporting path, not public retaliation. Digital well-being includes breaks, ergonomics, respectful communication, and awareness that shared devices may retain accounts or files.

**Questions with answers**

1. Does a router normally choose a destination based on a file name? No; it forwards packets using network-layer information such as IP prefixes.
2. Why can a browser reuse a DNS answer? A cached record may remain valid until its TTL expires.
3. What does TLS certificate hostname validation check? That the certificate is valid for the name the browser intended to contact.
4. What is the immediate response after entering a password on a suspected phishing site? Stop using that link, change the password through the official site, and report it through the trusted school channel.
5. Why is a backup not automatically safe? It can be exposed, corrupted, or infected; protect it and verify recovery works.
