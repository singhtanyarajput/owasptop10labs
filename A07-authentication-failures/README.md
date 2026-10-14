# A07:2025 – Authentication Failures

## 1. What the Vulnerability Is
Authentication Failures (formerly Identification and Authentication Failures) encompasses weaknesses in confirming a user's identity, verifying credentials, and maintaining session state. When authentication controls fail, attackers can compromise passwords, keys, or session tokens to assume other users' identities temporarily or permanently.

## 2. How It Works
Authentication systems are prime targets for automated attacks. Attackers exploit missing rate limits to launch credential stuffing or brute-force attacks, leverage predictable session token generation, exploit poorly protected "Remember Me" tokens, or abuse flawed password reset and multi-factor authentication (MFA) workflows.

## 3. Simple Illustrative Example
A mobile banking API provides a password reset endpoint:
```http
POST /api/auth/reset-password HTTP/1.1
Content-Type: application/json

{"email": "victim@example.com", "smsCode": "1234"}
```
The `smsCode` is a 4-digit numeric code (10,000 combinations). The endpoint enforces no rate limiting or lockout. An attacker launches an automated Burp Intruder attack iterating through `0000` to `9999` in less than two minutes, resets the victim's password, and takes over the account.

## 4. Common Causes
- Permitting automated brute-force attacks, credential stuffing, and password spraying due to missing rate limiting.
- Weak password complexity requirements and failing to check against compromised password lists.
- Flawed multi-factor authentication (MFA) implementation (e.g., optional MFA, MFA easily bypassed by dropping parameters).
- Insecure session management: predictable session IDs, session fixation, or failing to invalidate session tokens upon logout.
- Exposing session tokens in URLs or storing them insecurely in browser local storage.

## 5. Potential Impact
- Complete user and administrative account takeover.
- Unauthorized access to private customer data and transactional capabilities.
- Reputational damage and severe compliance sanctions.

## 6. How to Detect or Test It
- Testing login, registration, and reset endpoints with Burp Intruder to check for rate limiting, CAPTCHA, and account lockouts.
- Analyzing session token randomness, entropy, and lifecycle (verifying token invalidation upon logout or password reset).
- Testing MFA workflows for bypass opportunities (e.g., direct navigation to post-MFA endpoints, tampering with status codes).
- Testing username enumeration via response length, timing differences, or specific error messages.

## 7. How to Prevent or Fix It
- Enforce Multi-Factor Authentication (MFA) with FIDO2/WebAuthn or time-based one-time passwords (TOTP).
- Implement rate limiting, IP reputation filtering, and CAPTCHA on all authentication and recovery endpoints.
- Check new passwords against breached password databases (e.g., Have I Been Pwned API) and enforce NIST SP 800-63B guidelines.
- Use built-in, hardened session management frameworks; generate cryptographically secure 128-bit random session identifiers.
- Invalidate all existing sessions upon password reset, logout, or privilege changes.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A07:2025 – Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)
- **OWASP Cheat Sheet:** [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- **OWASP Cheat Sheet:** [Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- **PortSwigger Web Security Academy:**
  - [Authentication Vulnerabilities](https://portswigger.net/web-security/authentication)
  - [OAuth 2.0 Vulnerabilities](https://portswigger.net/web-security/oauth)
  - [JWT Attacks](https://portswigger.net/web-security/jwt)
