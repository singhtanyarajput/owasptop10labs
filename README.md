# OWASP Top 10:2025 Labs

Personal learning repository documenting hands-on laboratory exercises, vulnerability analyses, and defensive mitigations for the **OWASP Top 10:2025** application security risks.

## Categories Overview

| Identifier | Vulnerability Category | Description | Directory |
| :--- | :--- | :--- | :--- |
| **A01:2025** | Broken Access Control | Authorization bypasses, IDOR, SSRF, and privilege escalation | [A01-broken-access-control/](./A01-broken-access-control/) |
| **A02:2025** | Security Misconfiguration | Default settings, misconfigured headers, unhardened infrastructure | [A02-security-misconfiguration/](./A02-security-misconfiguration/) |
| **A03:2025** | Software Supply Chain Failures | Vulnerable dependencies, malicious packages, CI/CD pipeline risks | [A03-software-supply-chain-failures/](./A03-software-supply-chain-failures/) |
| **A04:2025** | Cryptographic Failures | Insecure encryption, sensitive data exposure, weak key handling | [A04-cryptographic-failures/](./A04-cryptographic-failures/) |
| **A05:2025** | Injection | SQLi, Command Injection, XSS, and interpreter manipulation | [A05-injection/](./A05-injection/) |
| **A06:2025** | Insecure Design | Business logic flaws, missing threat modeling, architectural weaknesses | [A06-insecure-design/](./A06-insecure-design/) |
| **A07:2025** | Authentication Failures | Credential stuffing, broken sessions, brute-force, weak MFA flows | [A07-authentication-failures/](./A07-authentication-failures/) |
| **A08:2025** | Software or Data Integrity Failures | Insecure deserialization, unverified updates, unvalidated critical data | [A08-software-or-data-integrity-failures/](./A08-software-or-data-integrity-failures/) |
| **A09:2025** | Security Logging and Alerting Failures | Missing logs, absent alerts, delayed incident detection and response | [A09-security-logging-and-alerting-failures/](./A09-security-logging-and-alerting-failures/) |
| **A10:2025** | Mishandling of Exceptional Conditions | Fail-open errors, unhandled exceptions, verbose stack traces, logic halts | [A10-mishandling-of-exceptional-conditions/](./A10-mishandling-of-exceptional-conditions/) |

## Repository Organization

Each category directory contains:
- `README.md`: Vulnerability overview, mechanics, illustrative examples, testing methodologies, remediation strategies, and official references.
- `labs/`: Individual markdown write-ups for completed hands-on lab exercises.
- `screenshots/`: Evidence captures and tool output referenced by lab write-ups.
