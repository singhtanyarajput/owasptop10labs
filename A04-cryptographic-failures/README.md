# A04:2025 – Cryptographic Failures

## 1. What the Vulnerability Is
Cryptographic Failures (previously categorized as Sensitive Data Exposure) refers to the absence or improper implementation of cryptography, resulting in the compromise of sensitive data such as credentials, payment card numbers, health records, personal identifiable information (PII), or session tokens.

## 2. How It Works
When data in transit or at rest is left unencrypted, protected with obsolete algorithms, encrypted with hardcoded or poorly generated keys, or hashed without salt and proper work factors, attackers can intercept network traffic, crack password databases, or forge signed tokens.

## 3. Simple Illustrative Example
A web application stores user passwords in its database using unsalted MD5 hashes:
```text
admin : 21232f297a57a5a743894a0e4a801fc3
```
An attacker who dumps the database passes the hash through a public rainbow table and instantly retrieves the plaintext password `admin`.

## 4. Common Causes
- Transmitting sensitive data over unencrypted channels (plain HTTP, unencrypted SMTP, outdated TLS 1.0/1.1).
- Utilizing deprecated cryptographic algorithms (MD5, SHA-1, DES, RC4, 3DES).
- Hardcoding encryption keys, initialization vectors (IVs), or JWT signing secrets directly in source code.
- Using standard pseudorandom number generators (e.g., `Math.random()`) instead of cryptographically secure random number generators (CSPRNG).
- Storing passwords without adaptive cryptographic hashing algorithms.

## 5. Potential Impact
- Mass exposure and compromise of user passwords and identity data.
- Financial fraud, regulatory penalties, and compliance failure (GDPR, HIPAA, PCI-DSS).
- Forgery of digital signatures, authentication tokens, and session states.

## 6. How to Detect or Test It
- Automated network inspection using tools like `testssl.sh` or Qualys SSL Labs to identify weak TLS ciphers and missing HSTS.
- Static application security testing (SAST) and code reviews to detect hardcoded secrets, weak cryptographic libraries, and broken pseudo-randomness.
- Intercepting HTTP traffic using Burp Suite to verify whether authentication cookies have the `Secure` flag and all sensitive requests enforce HTTPS.

## 7. How to Prevent or Fix It
- Classify all data processed by the application and identify sensitive information requiring protection.
- Encrypt all sensitive data at rest using strong, modern encryption (e.g., AES-256-GCM, ChaCha20-Poly1305) with secure key management.
- Enforce TLS 1.3 (or 1.2 minimum) for all data in transit and implement HTTP Strict Transport Security (HSTS).
- Hash passwords using modern, adaptive, salted functions with configurable work factors (Argon2id, bcrypt, scrypt, or PBKDF2).
- Never hardcode secrets in source code; use dedicated key management services (KMS) or secret vaults.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A04:2025 – Cryptographic Failures](https://top10.owasp.org/2025/A04_2025-Cryptographic_Failures/)
- **OWASP Cheat Sheet:** [Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- **OWASP Cheat Sheet:** [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- **PortSwigger Web Security Academy:**
  - [JWT (JSON Web Token) Attacks](https://portswigger.net/web-security/jwt)
  - [Information Disclosure](https://portswigger.net/web-security/information-disclosure)
