# Chapter 6 — Networks, Internet, ICT, and Digital Citizenship

**Course:** Computer Fundamentals and ICT  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–5

## What you will learn

You will distinguish a network from the Internet and the Web, identify basic network devices and protocols, trace a website request, explain email and cloud services, and apply basic privacy and cybersecurity principles in education and everyday life.

---

## 6.1 Data communication

A communication system sends data from a source to a destination.

~~~text
Sender → Transmitter → Communication medium → Receiver → Destination
~~~

A network needs:

- **Devices/nodes:** computers, phones, servers, printers, and network equipment.
- **Link/medium:** copper cable, fibre, or radio.
- **Protocol:** shared rules for formatting, sending, receiving, and interpreting data.
- **Addressing:** identifiers used to locate a destination or interface.
- **Error handling:** methods to detect or recover from lost or damaged data.

**Bandwidth** is a link's capacity, often measured in bits per second. **Throughput** is the data rate actually achieved. **Latency** is the time delay. A connection can have high bandwidth and still feel slow when latency is large.

---

## 6.2 Network scope

| Name | Typical scope | Example |
|---|---|---|
| PAN | Around one person | Phone and wireless earphones |
| LAN | Home, office, room, or school | Classroom computers |
| MAN | Metropolitan/city scale | City-wide campus network |
| WAN | Large geographic areas | A bank's nationwide network |

These are convenient categories; real networks can overlap and combine.

### Wired and wireless media

- **Twisted-pair copper:** common Ethernet cable for local networks.
- **Coaxial cable:** shielded cable used in some access networks.
- **Optical fibre:** transmits light and supports high capacity over distance.
- **Radio/Wi-Fi:** wireless local access; shared medium and interference can affect performance.
- **Cellular:** wide-area wireless service using provider infrastructure.

---

## 6.3 Network devices

- **Network interface card (NIC):** connects a device to a network and provides a link-layer interface.
- **Switch:** forwards local network frames using link-layer addresses.
- **Router:** forwards packets between IP networks.
- **Access point:** connects wireless clients to a local network.
- **Modem/ONT:** adapts a service-provider access link to a customer network.
- **Firewall:** applies rules to allow or block network traffic.

A home gateway often combines several functions. It can include a router, switch, Wi-Fi access point, firewall, and modem or ONT.

### Switch versus router

| Device | Main decision | Typical role |
|---|---|---|
| Switch | Which local interface is associated with a link-layer address? | Connect devices in one LAN |
| Router | Which next network should receive this IP packet? | Connect different IP networks |

The device descriptions are introductory. Details of frames, addressing, routing, and protocols are covered in the Computer Networks course.

---

## 6.4 Internet and World Wide Web

The **Internet** is a global interconnection of networks using the Internet Protocol suite. The **World Wide Web** is a service of linked resources that commonly uses browsers, URLs, HTTP, and HTTPS.

The Web is not the same as the Internet. Email, remote login, voice calls, file transfer, and many games can use the Internet without being Web pages.

### Client and server

- A **client** requests a resource or service.
- A **server** responds to requests and may serve many clients.
- A **protocol** defines the exchange.
- A **port number** identifies a service endpoint on a host; transport protocols and configuration determine actual usage.

A device can act as a client in one interaction and a server in another.

---

## 6.5 IP and MAC addresses

- An **IP address** identifies a network interface or destination in the Internet Protocol system. IPv4 uses 32-bit addresses; IPv6 uses 128-bit addresses.
- A **MAC address** is a link-layer address used on a local network. Its format and administration vary by technology.
- A **port number** helps identify an application service at a host.
- A domain name is a human-readable name; DNS helps resolve it to records such as IP addresses.

These identifiers work at different layers. A router forwards IP packets; a local Ethernet switch uses link-layer information to forward frames.

---

## 6.6 DNS, URL, HTTP, and HTTPS

### URL structure

Example:

~~~text
https://learn.example.org:443/notes/ch1?mode=print#summary
~~~

- Scheme: https
- Host/domain: learn.example.org
- Port: 443 (explicit here; often omitted when it is the default)
- Path: /notes/ch1
- Query: mode=print
- Fragment: summary

### DNS

The **Domain Name System (DNS)** is a distributed naming system. A resolver asks DNS servers for records associated with a domain. Results can be cached for a time. DNS does not download the web page; it helps locate the destination.

### HTTP and HTTPS

- **HTTP** defines web request and response messages.
- **HTTPS** is HTTP protected by TLS.
- TLS helps provide encryption, integrity, and authentication of the server identity.
- HTTPS does not prove that a site is truthful, safe, or operated by a trusted organisation. Check the domain and context.

### Website request sequence

~~~mermaid
sequenceDiagram
    participant B as Browser
    participant D as DNS resolver
    participant S as Web server
    B->>D: Resolve domain name
    D-->>B: Return DNS record
    B->>S: Connect and send HTTPS request
    S-->>B: Return response and page resources
    B->>B: Interpret HTML, CSS, and JavaScript
    B-->>B: Render page to user
~~~

HTML describes page structure, CSS styles the page, and JavaScript commonly adds behavior.

---

## 6.7 Email and other Internet services

- **SMTP:** commonly used to submit or relay outgoing email.
- **IMAP:** synchronises and manages email stored on a mail server.
- **POP3:** retrieves email from a server, with retention behavior controlled by settings.
- **FTP/SFTP:** file transfer protocols; SFTP is a separate protocol carried over SSH, not simply FTP with a security switch.
- **DNS:** name and service records.
- **HTTP/HTTPS:** Web resources.
- **TCP:** connection-oriented transport with reliability and ordering features.
- **UDP:** connectionless transport with fewer built-in delivery guarantees.

The full protocol stack and numerical problems belong in the Computer Networks chapter course.

---

## 6.8 Cloud computing

Cloud computing supplies configurable computing resources over a network.

Common service models:

- **SaaS:** software used as a service, such as hosted email.
- **PaaS:** managed platform to deploy applications.
- **IaaS:** virtual machines, storage, or networking as infrastructure.

Cloud services can reduce local maintenance and scale resources, but they still depend on provider availability, connectivity, access control, data location, backups, and cost management.

---

## 6.9 ICT in education and public services

ICT supports:

- **Teaching:** presentations, simulations, digital textbooks, and accessible resources.
- **Learning:** self-paced practice, recorded lessons, and collaboration.
- **Assessment:** quizzes, assignments, feedback, and progress reports.
- **Administration:** attendance, schedules, records, and communication.
- **Public services:** online applications, certificates, payments, and notices.

### Digital inclusion

A digital tool is not automatically accessible to every student. Consider:

- availability and affordability of devices and data;
- reliable electricity and connectivity;
- language and digital literacy;
- disability access, including captions and screen-reader support;
- privacy and age-appropriate use;
- offline alternatives and support for shared devices.

A teacher remains responsible for selecting suitable tools, checking content, protecting student data, and using results fairly.

---

## 6.10 Cybersecurity and digital citizenship

### CIA security goals

- **Confidentiality:** only authorised people can read the information.
- **Integrity:** information stays correct and changes are authorised.
- **Availability:** authorised users can access systems when required.

### Threats

- **Phishing:** deceptive message or page intended to steal data or trigger an unsafe action.
- **Malware:** harmful software such as ransomware, spyware, or a trojan.
- **Social engineering:** manipulation of a person into revealing information or granting access.
- **Credential reuse:** one stolen password opens multiple accounts.
- **Data loss:** accidental deletion, hardware failure, theft, or attack.

### Practical protections

- Use long, unique passwords and a password manager.
- Enable multi-factor authentication where possible.
- Check the sender and domain before opening links or attachments.
- Keep operating systems and applications updated.
- Restrict access to the information each person needs.
- Keep separate backups and test restoration.
- Avoid posting sensitive personal details publicly.
- Ask permission before sharing another person's photo or data.
- Report harmful messages instead of forwarding them.

### Digital citizenship

Responsible participation includes respect, privacy, source evaluation, copyright awareness, accessibility, and preventing cyberbullying. A professional teacher models safe and constructive online behavior.

---

## 6.11 Worked examples

**1. Distinguish the services.** A school opens a web page, sends an email, and joins a video call.  
**Answer:** All may use the Internet. The page uses Web protocols; email commonly uses SMTP plus IMAP/POP for mailbox access; the video service may use multiple application and transport protocols.

**2. Convert data rate.** A 40 Mb/s link has an ideal maximum of how many MB/s?  
~~~text
40 megabits/s ÷ 8 bits/byte = 5 megabytes/s
~~~
Actual file transfer is lower due to protocol overhead and other limits.

**3. Which device?** A packet must move from the school LAN to an ISP network.  
**Answer:** A router forwards it between IP networks. A switch forwards local frames within the LAN.

**4. Is HTTPS enough to trust a page?**  
**Answer:** No. It protects the connection to the named site; it does not prove that the operator's claims or content are trustworthy.

---

## 6.12 Questions and answers

1. **What is a protocol?**  
   Agreed rules for communication and data interpretation.

2. **How do bandwidth and latency differ?**  
   Bandwidth is capacity; latency is delay.

3. **How do a switch and router differ?**  
   A switch mainly connects local devices; a router forwards packets between IP networks.

4. **What does DNS do?**  
   It resolves names and returns DNS records used to locate services.

5. **What is the Web?**  
   A system of linked resources accessed over the Internet using Web protocols.

6. **What does HTTPS protect?**  
   The HTTP connection using TLS, including encryption and integrity protection.

7. **What does SMTP commonly do?**  
   Sends or relays email.

8. **What is cloud computing?**  
   On-demand use of computing resources delivered over a network.

9. **Name the CIA triad.**  
   Confidentiality, integrity, and availability.

10. **Give one digital-inclusion consideration for teaching.**  
    Device access, connectivity, language, disability access, affordability, privacy, or offline alternatives.

### Multiple-choice check

1. DNS commonly helps find: A. IP-related records for a domain B. a CPU register C. file contents D. a keyboard driver  
2. A router mainly forwards: A. packets between networks B. pixels to a display C. local files D. compiler tokens  
3. HTTPS uses: A. TLS B. OCR C. BCD D. DMA  
4. Which is commonly an email sending protocol? A. SMTP B. IMAP only C. HTML D. Ethernet  
5. Confidentiality means: A. authorised access to information B. all data is public C. no backups D. faster CPU  
6. 80 Mb/s ideal equals: A. 1 MB/s B. 8 MB/s C. 10 MB/s D. 80 MB/s

**Answers:** 1-A, 2-A, 3-A, 4-A, 5-A, 6-C.

### Draw-from-memory tasks

- Draw a client resolving a name through DNS, then making an HTTPS request to a server.
- Draw a switch inside one LAN and a router connecting two networks.
- Draw the CIA triad and write one classroom example beside each term.
