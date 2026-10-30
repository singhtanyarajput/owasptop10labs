A01 — Broken Access Control
Your Access Control labs are completed
Primary PortSwigger topic to cover

- Access control vulnerabilities

Related topics to revisit for access-control overlap
- SSRF — OWASP 2025 explicitly includes SSRF under A01, though SSRF still deserves its own practical study.
- API testing — authorization checks on API endpoints and objects.
- GraphQL API vulnerabilities — unauthorized queries and mutations.
- Web cache deception — where sensitive information may be exposed due to cache and authorization interactions.
- Business logic vulnerabilities — unauthorized actions or workflows.
What to do now: keep A01 marked complete for the Access Control topic. When you later study these related topics, add cross-references to A01 where appropriate rather than duplicating entire lab write-ups.
A02 — Security Misconfiguration
Study these PortSwigger topics
- Security-related issues encountered through Information disclosure
- CORS
- Clickjacking
- HTTP Host header attacks
- Web cache poisoning
- Web cache deception
- WebSockets — insecure configuration and origin validation
- File upload vulnerabilities — unsafe server configuration
- API testing — exposed endpoints and permissive configurations
These are not all exclusively A02. For example, CORS misconfiguration is a strong fit, but a file-upload lab that executes uploaded code may primarily involve other weaknesses. Classify each lab according to its actual root cause.
A03 — Software Supply Chain Failures
Study these topics where relevant
- Insecure deserialization — especially when exploitation depends on untrusted or compromised components or artifacts
- Prototype pollution — particularly when caused by vulnerable dependencies
- Web LLM attacks — where external integrations, plugins, or dependent components create supply-chain risks
- Essential skills — use for dependency and component assessment if you extend beyond the labs
Important: PortSwigger does not appear to have a dedicated, comprehensive Software Supply Chain Failures lab collection. Its general vulnerability labs cannot substitute for learning dependency compromise, build-system security, package integrity, and software provenance. This category will need additional resources beyond the PortSwigger topic list.
A04 — Cryptographic Failures
Study these topics
- Authentication — password storage and cryptographic weaknesses where specifically demonstrated
- JWT — weak signing or verification and algorithm-handling flaws
- OAuth authentication — token handling and secure communication where cryptographic controls are relevant
- Information disclosure — only when sensitive data exposure is caused by a cryptographic failure
PortSwigger has no broad standalone Cryptographic Failures lab category in the list you sent. Supplement this category with study of TLS, encryption at rest, key management, password hashing, and secure randomness.
A05 — Injection
This is one of the biggest practical sections.

Study these PortSwigger topics
- SQL injection
- Cross-site scripting (XSS)
- DOM-based vulnerabilities
- OS command injection
- Server-side template injection (SSTI)
- XML external entity (XXE) injection
- NoSQL injection
- Path traversal — particularly when untrusted input changes file-system paths
- Web LLM attacks — prompt injection is a related concept, but don't automatically classify every LLM lab as traditional A05 injection
- Prototype pollution — when attacker-controlled properties reach dangerous sinks
- GraphQL API vulnerabilities — if user-controlled inputs lead to injection
- HTTP request smuggling — where input parsing discrepancies are the primary flaw
Study order: SQL injection → XSS → OS command injection → path traversal → NoSQL injection → XXE → SSTI → DOM-based vulnerabilities → remaining advanced topics.
A06 — Insecure Design
Study these PortSwigger topics
- Business logic vulnerabilities
- Race conditions
- Authentication — design flaws in login, account recovery, or multi-factor workflows
- Access control — flawed permission models or authorization design
- API testing — flawed business workflows and missing security requirements
- Web LLM attacks — where the core issue is a missing safety or trust-boundary design rather than injection alone
Focus on failures in the design of a feature or workflow, rather than a simple coding mistake. Business logic and race-condition labs are especially useful here.
A07 — Authentication Failures
Study these PortSwigger topics
- Authentication
- OAuth authentication
- JWT
- WebSockets — authentication and session handling where relevant
- API testing — token and session verification
- Cross-site request forgery (CSRF) — related to session abuse, although CSRF is not inherently an authentication failure
- Information disclosure — when leaked credentials or reset tokens cause an authentication weakness
This is the next distinct category you should study after A02–A06 in your OWASP learning sequence. Don't put all Authentication labs under A01 just because access control and authentication are related.
A08 — Software or Data Integrity Failures
Study these PortSwigger topics
- Insecure deserialization
- File upload vulnerabilities — when the flaw involves trusting unverified or tampered files
- JWT — when the problem is failure to verify token integrity
- OAuth authentication — when the issue involves trusting unvalidated data or tokens
- Web cache poisoning — when the attack corrupts data or content that downstream systems trust
- Prototype pollution — when untrusted properties alter application behavior
For this category, concentrate on trust boundaries, integrity verification, signed data, and unsafe deserialization. Some of these topics overlap A03, but A03 is broader and focuses on the software supply chain.
A09 — Security Logging and Alerting Failures
Study these PortSwigger topics
- Information disclosure — useful for understanding what data is exposed, but not a direct substitute for logging labs
- Business logic vulnerabilities — useful for constructing abuse scenarios that should be detected
- API testing — useful for identifying sensitive actions that require audit trails
- Race conditions — useful for thinking about event visibility and incident reconstruction
Coverage gap: the PortSwigger topics shown do not provide a dedicated, comprehensive A09 lab collection. You will need additional practice with audit logging, alert generation, event correlation, log tampering, and incident detection.
A10 — Mishandling of Exceptional Conditions
Study these PortSwigger topics
- Information disclosure — verbose errors and stack traces
- Business logic vulnerabilities — unexpected states and invalid transitions
- Race conditions — failures under concurrent requests
- Insecure deserialization — exceptions caused by malformed or unexpected serialized data
- SQL injection, NoSQL injection, and other injection topics — error handling where relevant
- HTTP request smuggling — inconsistent parsing and failure handling
- API testing — malformed requests and unexpected application states
Coverage gap: these topics provide useful exercises, but they do not cover every A10 scenario. Focus on improper error handling, fail-open behavior, unhandled exceptions, invalid state transitions, and recovery behavior. Don't classify every error message as A10; an error that reveals a database query, for instance, may be better documented under information disclosure or injection.

Your actual study order from today
Since you've completed all the Access Control labs, use this sequence for the next phase.
Order	OWASP category	PortSwigger topics to work through
1	A01 — Broken Access Control	Completed: Access Control
2	A02 — Security Misconfiguration	CORS, Clickjacking, HTTP Host header attacks, Web cache poisoning, Web cache deception
3	A03 — Software Supply Chain Failures	Supplement PortSwigger with dependency, package, and build-system security
4	A04 — Cryptographic Failures	JWT and relevant cryptographic weaknesses, supplemented with dedicated theory and exercises
5	A05 — Injection	SQL injection, XSS, command injection, SSTI, XXE, NoSQL injection, and related topics
6	A06 — Insecure Design	Business logic vulnerabilities, Race conditions
7	A07 — Authentication Failures	Authentication, OAuth authentication, relevant JWT labs
8	A08 — Software or Data Integrity Failures	Insecure deserialization, relevant JWT and file-upload labs
9	A09 — Security Logging and Alerting Failures	Supplement with logging, alerting, monitoring, and incident-detection exercises
10	A10 — Mishandling of Exceptional Conditions	Error handling, unexpected states, malformed inputs, and failure scenarios across relevant labs