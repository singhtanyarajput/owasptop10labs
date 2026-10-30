# A02:2025 – Security Misconfiguration

## 1. What the Vulnerability Is
OWASP A02: Security Misconfiguration happens when an application's security settings are defined, implemented, or maintained incorrectly.

Think of it like buying a heavy-duty safe for your valuables, but leaving the factory default combination as 0-0-0-0, or forgetting to lock it entirely because the locking mechanism was too complicated to figure out. The security features exist, but because they are configured poorly or left on default settings, they provide no actual protection.
Security Misconfiguration occurs when security controls, cloud infrastructure, web servers, databases, or application frameworks are inaccurately configured, left at default settings, or maintained with unnecessary exposed features. In the OWASP Top 10:2025, it holds the **#2 position**.

## 2. How It Works
Modern applications rely on complex, multi-layered stacks (cloud providers, containers, web servers, runtime environments, databases). Attackers scan for default credentials, enabled debugging endpoints, open cloud storage buckets, verbose error pages, or missing security headers to gain initial access, extract sensitive data, or pivot across the infrastructure.

## 3. Simple Illustrative Example
A Spring Boot application is deployed to production with debugging actuator endpoints exposed:
```http
GET /actuator/env HTTP/1.1
Host: target.example.com
```
Because actuator endpoints were not restricted, the server returns environment variables containing database connection strings, AWS access keys, and internal API credentials.

## 4. Common Causes
- Default accounts with well-known passwords enabled (e.g., admin/admin, default DBMS accounts).
- Verbose error messages enabled in production, returning full stack traces, database schema details, and software versions.
- Missing or misconfigured HTTP security headers (`Content-Security-Policy`, `X-Content-Type-Options`, `Strict-Transport-Security`).
- Unnecessary ports, services, sample applications, or administrative utilities left open to the internet.
- Overly permissive cloud storage bucket policies (e.g., public read/write AWS S3 buckets).

## 5. Potential Impact
- Exposure of administrative consoles and internal management interfaces.
- Leakage of intellectual property, proprietary source code, and credentials.
- Full server or cloud environment compromise leading to lateral network movement.

## 6. How to Detect or Test It
- Automated vulnerability scanning using tools like OWASP ZAP, Nikto, or Burp Suite Scanner to detect missing headers and exposed default endpoints.
- Directory and endpoint fuzzing (e.g., using ffuf, Gobuster) targeting common management panels, `.git` repositories, backup files (`.bak`, `.old`), and Swagger/OpenAPI docs.
- Cloud security posture reviews inspecting S3 permissions, IAM roles, and security groups.

## 7. How to Prevent or Fix It
- Establish a repeatable, automated hardening process for all environments (development, staging, and production).
- Remove or disable all unused features, components, services, and sample applications.
- Enforce strict HTTP security headers across all web servers and reverse proxies.
- Ensure error handling returns generic, standardized messages while recording debug information only in secured internal logs.
- Regularly audit configurations using automated Infrastructure-as-Code (IaC) scanners and cloud compliance tooling.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A02:2025 – Security Misconfiguration](https://top10.owasp.org/2025/A02_2025-Security_Misconfiguration/)
- **OWASP Cheat Sheet:** [Configuration and Deployment Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Configuration_and_Deployment_Management_Cheat_Sheet.html)
- **PortSwigger Web Security Academy:**
  - [Information Disclosure](https://portswigger.net/web-security/information-disclosure)
  - [CORS Misconfigurations](https://portswigger.net/web-security/cors)
  - [XML External Entity (XXE) Injection](https://portswigger.net/web-security/xxe)
  - [File Upload Vulnerabilities](https://portswigger.net/web-security/file-upload)
