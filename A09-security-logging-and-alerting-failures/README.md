# A09:2025 – Security Logging and Alerting Failures

## 1. What the Vulnerability Is
Security Logging and Alerting Failures occurs when an application fails to generate adequate audit logs, omits critical security event telemetry, stores logs insecurely, or fails to alert administrators when suspicious or malicious actions occur. Without logging and real-time alerting, breaches can continue undetected for months.

## 2. How It Works
When attackers probe an application, perform brute-force attacks, or test injection payloads, their actions generate errors and abnormal requests. If the application suppresses error logs, fails to record failed logins, stores logs without context, or lacks threshold alerts, security operations teams have zero visibility into active intrusion attempts.

## 3. Simple Illustrative Example
An attacker runs automated credential stuffing against `/api/login`, generating 50,000 failed login attempts over four hours from a rotating proxy network.
Because the application does not log failed authentication attempts or track failed login thresholds, no alert is triggered. The attacker successfully identifies 45 valid user accounts without alerting anyone until compromised users report unauthorized charges weeks later.

## 4. Common Causes
- Auditable security events (failed logins, access control denials, input validation errors) are not logged.
- Logs contain insufficient detail (e.g., missing timestamps, client IP addresses, user IDs, or affected resources).
- Logs are stored only on the local web server filesystem and overwritten or deleted if the server restarts or crashes.
- Absence of real-time monitoring and alerting thresholds connected to a Security Information and Event Management (SIEM) system.
- Log injection vulnerabilities where user input containing newline characters (`\r\n`) corrupts log formatting.

## 5. Potential Impact
- Undetected data exfiltration and persistent network compromise spanning months or years.
- Inability to perform post-incident digital forensics to determine the scope of a breach.
- Non-compliance with legal and regulatory mandates (GDPR, PCI-DSS, HIPAA, SOC 2).

## 6. How to Detect or Test It
- Perform deliberate security test events (e.g., intentional failed logins, SQL injection test probes, unauthorized resource access) and verify whether logs are generated with accurate context.
- Verify whether log alerts trigger automated notifications (Slack, email, PagerDuty, SIEM alert) when thresholds are breached.
- Test for log injection by submitting CRLF characters in inputs (e.g., username `admin\nINFO: User admin logged in successfully`).

## 7. How to Prevent or Fix It
- Log all authentication failures, access control failures, server-side input validation errors, and critical business transactions.
- Format logs consistently (e.g., structured JSON) with necessary context: timestamp, origin IP, user identity, correlation ID, and event type.
- Sanitize all logged data to prevent log injection and CRLF attacks; never log passwords, session tokens, or payment card data.
- Stream logs securely to a centralized, write-once, tamper-resistant logging platform / SIEM (e.g., Elasticsearch, Splunk).
- Configure automated alerting for anomalous spikes in error rates, failed authentications, or privilege changes.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A09:2025 – Security Logging and Alerting Failures](https://top10.owasp.org/2025/A09_2025-Security_Logging_and_Alerting_Failures/)
- **OWASP Cheat Sheet:** [Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- **PortSwigger Research & Testing:**
  - [Application Security Testing & Out-of-band Detection](https://portswigger.net/burp/application-security-testing/oast)
