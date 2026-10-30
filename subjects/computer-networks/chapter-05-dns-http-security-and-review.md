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
