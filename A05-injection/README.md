# A05:2025 – Injection

## 1. What the Vulnerability Is
Injection vulnerabilities occur when untrusted data is sent to an interpreter as part of a command, query, or script without proper validation, sanitization, or parameterization. The interpreter processes the hostile input as instructions, executing arbitrary commands or altering data access. This category includes SQL Injection, Cross-Site Scripting (XSS), OS Command Injection, NoSQL Injection, and Server-Side Template Injection (SSTI).

## 2. How It Works
Applications frequently concatenate user input directly into structured command or query strings. When an attacker supplies characters that carry semantic meaning in the target language (such as `'`, `"`, `;`, `--`, `<`, `{{`), they break out of the data context and transition into the command context, altering the parser's logic.

## 3. Simple Illustrative Example
### SQL Injection
A web service performs user lookup via direct string concatenation:
```python
query = "SELECT * FROM users WHERE username = '" + user_input + "' AND password = '" + pass_input + "'"
```
If an attacker inputs `admin' --` into the username field, the executed query becomes:
```sql
SELECT * FROM users WHERE username = 'admin' --' AND password = '...'
```
The query verifies only the username `admin` and comments out the password condition, granting immediate unauthorized login.

## 4. Common Causes
- Directly concatenating or interpolating untrusted input into SQL queries, system shells, or LDAP lookups.
- Failing to use parameterized queries, prepared statements, or safe Object-Relational Mappers (ORMs).
- Directly reflecting untrusted data into HTML responses without context-aware output encoding (XSS).
- Passing user input directly to server-side template engines or eval functions.

## 5. Potential Impact
- Full database exfiltration, unauthorized modification, or total data deletion.
- Remote Code Execution (RCE) and full operating system compromise via Command Injection.
- Client-side account takeover, session hijacking, and defacement via Cross-Site Scripting (XSS).

## 6. How to Detect or Test It
- Manual payload testing in Burp Suite using targeted probe characters (`'`, `"`, `|`, `&`, `{{7*7}}`, `<script>`).
- Automated dynamic testing using tools like SQLMap, OWASP ZAP, and Burp Suite Scanner.
- Static code analysis (SAST) scanning for raw query string construction and dangerous execution sinks (`exec()`, `system()`, `eval()`).

## 7. How to Prevent or Fix It
- Use parameterized queries (prepared statements) for all database interactions.
- Avoid invoking system shell interpreters; use built-in language APIs with argument arrays instead of shell execution.
- Implement context-sensitive output encoding (HTML, JavaScript, CSS, URL encoding) before rendering data in the browser.
- Use secure templating engines configured to auto-escape HTML output by default.
- Apply strict allowlist input validation for expected formats, lengths, and character sets.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A05:2025 – Injection](https://top10.owasp.org/2025/A05_2025-Injection/)
- **OWASP Cheat Sheet:** [SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- **OWASP Cheat Sheet:** [Cross-Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- **PortSwigger Web Security Academy:**
  - [SQL Injection (SQLi)](https://portswigger.net/web-security/sql-injection)
  - [Cross-Site Scripting (XSS)](https://portswigger.net/web-security/cross-site-scripting)
  - [OS Command Injection](https://portswigger.net/web-security/os-command-injection)
  - [NoSQL Injection](https://portswigger.net/web-security/nosql-injection)
  - [Server-Side Template Injection (SSTI)](https://portswigger.net/web-security/server-side-template-injection)
