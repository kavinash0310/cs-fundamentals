# Chapter 5 — DNS, HTTP, Security, and Review

## 1. DNS and web request
DNS maps names to records such as A/AAAA addresses. A resolver may query recursively or iteratively and cache answers for TTL. CNAME aliases another name; MX identifies mail exchangers.

When a browser opens a URL: resolve name, establish transport (TCP/TLS for common HTTPS or QUIC/HTTP3), send HTTP request, receive response, fetch dependent resources, render. Proxies/CDNs may change path.

## 2. HTTP and HTTPS
HTTP is application request/response protocol. Methods include GET, POST, PUT, DELETE; status codes group success (2xx), redirection (3xx), client error (4xx), server error (5xx). Cookies and authorization carry application state; HTTP itself is stateless.

HTTPS is HTTP protected by TLS. Certificates bind identities to public keys through a trust chain. TLS provides confidentiality, integrity, and authentication subject to correct validation.

## 3. Basic security concepts
Symmetric encryption uses shared secret; public-key cryptography uses key pairs. A digital signature signs a hash with private key and verifies with public key; it provides authenticity/integrity, not confidentiality. Firewalls filter traffic by rules. Authentication proves identity; authorization determines allowed actions.

## 4. Review questions
**Q1.** What does DNS TTL affect? **A:** Cache lifetime.
**Q2.** TCP vs HTTP? **A:** Transport versus application protocol.
**Q3.** What does a signature provide? **A:** Integrity and signer authentication, not secrecy.
**Q4.** Which HTTP method commonly retrieves? **A:** GET.
Practice: trace a website request; interpret status codes; compare symmetric/asymmetric crypto; explain certificate validation.

## Notebook work
Draw DNS resolution and an HTTPS request across application, transport, internet, and link layers.


## DNS and TLS request trace

A stub resolver asks a recursive resolver; cache may answer immediately. Otherwise resolver follows root, TLD, and authoritative referrals. A/AAAA records map names to IPv4/IPv6 addresses; TTL limits cache lifetime. DNS uses UDP commonly, with TCP for cases such as large/truncated responses or zone transfer; encrypted DNS variants exist.

HTTPS trace: resolve hostname; establish transport; perform TLS handshake, validate certificate name/trust chain, negotiate keys; send HTTP request encrypted; receive response. SNI may reveal hostname in some TLS configurations; encrypted ClientHello changes that exposure.

A digital signature verifies possession of a private key and data integrity, but does not encrypt the message. Symmetric encryption is efficient for bulk data; public-key cryptography helps establish/agree keys and identities.

**Practice:** Explain why a DNS cache can briefly return a stale address; distinguish authentication from authorization; identify the purpose of certificate hostname validation.

## DNS, HTTP, and TLS details

DNS caching reduces repeated lookup but stale data may persist until TTL expiry. Recursive resolver performs work on behalf of stub; authoritative server provides zone data. DNSSEC signs DNS data for authenticity/integrity but does not encrypt queries.

HTTP methods express intent; idempotency is about repeating an operation having the same intended effect. GET is safe/idempotent by semantics; PUT is typically idempotent; POST may create repeated effects unless application uses an idempotency key.

TLS certificate validation checks chain to a trusted root, validity period, and hostname match. Encryption without authentication can allow a man-in-the-middle. HSTS can require HTTPS for a domain after policy is learned.

**Practice:** Trace DNS cache hit/miss; explain 301 vs 404 vs 500; distinguish TLS certificate authentication from HTTP authorization; explain why a valid certificate does not prove a site is benevolent.

## Extended lecture: follow a web request

When a browser opens https://shop.example, it first needs a destination address. A typical DNS lookup checks browser/OS caches, then a recursive resolver. The resolver may query root, top-level-domain, and authoritative servers; cached records have TTLs, so the answer can be reused until expiry. DNS maps names to records such as A (IPv4), AAAA (IPv6), CNAME (alias), MX (mail), and TXT.

After resolving an address, the client establishes transport. Traditional HTTPS uses TCP plus TLS; HTTP/3 uses QUIC over UDP. TLS authenticates the server using certificates and negotiates keys, then encrypts and integrity-protects application traffic. DNS resolution itself is not automatically encrypted unless a protected DNS transport is used.

~~~text
URL -> DNS answer -> transport connection -> TLS handshake
    -> HTTP request -> response bytes -> browser rendering
~~~

HTTP is stateless at the protocol level: each request carries its method, target, headers, and optional body. Cookies, bearer tokens, or other state mechanisms connect requests into a logged-in session. GET should retrieve a representation and be safe to retry; POST usually submits or changes data and is not automatically idempotent. A status code class communicates outcome: 2xx success, 3xx redirection, 4xx client-side issue, 5xx server-side failure.

### Security: name the property

- Confidentiality: unauthorized observers cannot read the data.
- Integrity: unauthorized changes are detected or prevented.
- Authentication: identify the peer or user.
- Authorization: decide what an authenticated identity may do.
- Availability: keep the service usable under faults or attack.

TLS can provide confidentiality and integrity in transit and authenticate a server when certificate validation succeeds. It does not prove that a site is honest, secure after decryption, or authorized to access every resource. A valid certificate is not a guarantee that the business is trustworthy.

### Interview and exam prompts

1. Why can DNS return a stale answer? A cache may retain a record until its TTL expires; operational propagation is not instantaneous.
2. What does TLS protect? The connection's confidentiality and integrity, plus peer authentication under its trust model.
3. Why are cookies not part of HTTP's core state? HTTP requests are independent; cookies are an application mechanism sent in headers.
4. What is the difference between authentication and authorization? Identity verification versus permission checking.

## 5. Mail and file-transfer protocols

Application protocols define message formats and the actions peers perform. Know the direction and purpose before memorizing port numbers.

- SMTP transfers outgoing mail from a client or mail server toward a mail server. Mail submission often uses authentication and transport protection; server-to-server delivery also uses SMTP.
- POP3 retrieves messages from a mailbox. Its simple model is download-oriented; implementations can leave copies on the server, but POP3 is not the same as IMAP's synchronized multi-folder mailbox model.
- FTP transfers files over separate control and data connections. Traditional FTP does not encrypt credentials or content. FTPS adds TLS to FTP; SFTP is a distinct file-transfer protocol carried over SSH, not “secure FTP” in the FTP protocol family.
- HTTP transfers web representations and APIs; SMTP, POP3, and FTP have different message and session semantics.

~~~text
Sender --SMTP--> outgoing mail server --SMTP--> recipient mail server
                                                   |
Client --POP3 retrieval-----------------------------+
File client --FTP control + data connections--> FTP server
~~~

A socket API gives a program an endpoint for transport communication. A server commonly binds a local address/port and listens; a client connects to a server endpoint. TCP provides a reliable byte stream, while UDP sends datagrams without built-in delivery or ordering guarantees. The application protocol still decides how messages are framed and interpreted.

**Questions:** Which protocol retrieves mail in a basic download-oriented model? POP3. Which transfers outgoing mail? SMTP. Is SFTP a mode of FTP? No; it is a separate protocol over SSH.

## 6. Wi-Fi: shared wireless LAN

Wi-Fi refers to wireless local networking based on IEEE 802.11 families. A station scans for a network, authenticates as configured, and associates with an access point (AP); the AP bridges wireless clients to the distribution network. An infrastructure network uses an AP; an ad hoc network can allow stations to communicate directly when supported.

Wireless stations share a radio channel and cannot reliably detect collisions while transmitting in the same way a wired Ethernet station can. 802.11 therefore uses collision avoidance: a station senses the medium, waits according to contention rules, and acknowledges successful unicast frames. Hidden terminals can interfere even when two senders cannot hear one another; optional request-to-send/clear-to-send exchange can reduce some such collisions at the cost of overhead.

~~~text
Laptop ))))\
Phone  ))))) >  Access Point ---- switch/router ---- Internet
Tablet ))))/
~~~

Encryption and authentication protect access and traffic, but a familiar network name is not proof of a trustworthy AP. Prefer modern supported security configured by the network owner, verify the network identity, and use end-to-end TLS for sensitive application traffic. A Wi-Fi password protects network access; it does not replace HTTPS or application authorization.

## 7. Public-key cryptography, signatures, certificates, and firewalls

Symmetric encryption uses one shared secret for encryption and decryption; it is efficient for bulk data. Public-key cryptography uses a key pair. In a basic public-key encryption model, a sender can encrypt to a recipient's public key and only the matching private key decrypts. Real protocols combine public-key operations or key agreement with symmetric session keys rather than encrypting all bulk traffic directly with public-key operations.

A digital signature is different from encryption. The signer computes a digest of the data and signs it with a private signing key; a verifier checks the signature using the corresponding public key and recomputes the digest. This can establish integrity and evidence of key possession under a trust model. It does not hide the message.

A certificate is a signed binding between an identity (such as a DNS name) and a public key, issued by a certificate authority. A client validates the chain to a trusted root, validity period, intended usage, and the exact hostname. Revocation status and trust-store policy matter too. A valid certificate proves neither that the organization is honest nor that its application is safe.

A firewall applies traffic rules at a network boundary or host:

- Stateless packet filters match fields such as addresses, protocol, and port.
- Stateful firewalls track connection state and allow traffic consistent with established flows.
- Application proxies can inspect or mediate application-level exchanges.

Use least privilege: allow only required flows and log decisions appropriately. Firewalls reduce exposure but cannot stop every threat, especially malicious content delivered over allowed connections or compromised endpoints.

~~~text
Private signing key -> signs digest + message
Public verification key -> verifies signature
Certificate Authority -> signs certificate binding name to public key
Firewall -> permits or blocks traffic according to policy
~~~

### Integrated review

1. A mail client sends a message: SMTP. It retrieves a mailbox in a basic download model: POP3.
2. A file-transfer diagram shows distinct control and data channels: FTP. If the link says SSH file transfer, it is SFTP, not FTP with a flag.
3. A signature is valid but the content is visible: expected, because signatures provide integrity/authenticity properties, not confidentiality.
4. A certificate is valid for example.net but the browser visits other.example: hostname validation must fail.
5. A public Wi-Fi AP uses a familiar SSID: the name alone does not authenticate the AP.
6. A firewall allows outbound HTTPS: it does not establish that every HTTPS destination is safe.

This section extends beyond the GATE 2027 computer-network topic list. It is useful broader CS preparation; BPSC teacher candidates must follow the exact current teacher notice for their post and level.
