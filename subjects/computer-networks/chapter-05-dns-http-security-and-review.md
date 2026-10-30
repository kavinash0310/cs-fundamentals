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