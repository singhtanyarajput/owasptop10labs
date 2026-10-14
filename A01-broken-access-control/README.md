# A01:2025 – Broken Access Control

Broken Access Control maintains the **#1 position** in the OWASP Top 10:2025. It represents the failure of an application to properly enforce restrictions on what authenticated or unauthenticated users are allowed to do and which resources they are permitted to access.

---

## 1. What the Vulnerability Is

Access control (authorization) is the process by which an application grants or denies specific users the ability to access specific resources or execute specific actions. 

While **Authentication** verifies *who a user is*, **Access Control** determines *what a user is allowed to do*. 

Broken Access Control occurs whenever an attacker can access, modify, or delete resources or functionality outside their authorized permissions. This encompasses:
- **Horizontal Privilege Escalation:** Accessing resources belonging to another user with identical privilege levels (e.g., User A viewing User B's billing invoice).
- **Vertical Privilege Escalation:** Accessing higher-privileged resources or administrative functions from a lower-privileged account (e.g., a standard user accessing `/admin/users/delete`).
- **Context-Dependent & State Bypasses:** Skipping mandatory workflow steps (e.g., completing checkout without payment).
- **Server-Side Request Forgery (SSRF):** Forcing the backend server to make unauthorized requests to internal network resources, services, or metadata endpoints.
- **Cross-Origin Resource Sharing (CORS) Misconfigurations:** Permitting unauthorized origins to read sensitive authenticated responses.

---

## 2. How It Works

Access control enforcement is fundamentally a server-side responsibility. Broken access control typically arises when:
1. **Client-Side Reliance:** The application hides or disables UI components (like an "Admin Panel" link) in the browser, but fails to validate user permissions when the backend API receives the direct HTTP request.
2. **Parameter Tampering / IDOR:** The backend takes an identifier (e.g., `id`, `user_id`, `account_number`) directly from user input (query string, JSON body, or cookie) and fetches the record directly from the database without verifying that the authenticated session owns that record.
3. **Inconsistent Routing Rules:** Frameworks or web servers apply access rules based on URL patterns (e.g., `/admin/*`), but alternative HTTP methods (`POST` vs `GET`), path normalization quirks (`/admin/..;/users`), or custom headers (`X-Original-URL`, `X-Rewrite-URL`) bypass the filter.
4. **Missing Attribute or Role Checks:** Middleware checks if a user is logged in (`isAuthenticated()`), but neglects to verify whether the user has the requisite role or attribute (`hasRole('ADMIN')` or `record.ownerId == session.userId`).

---

## 3. Simple Illustrative Examples

### Example A: Insecure Direct Object Reference (Horizontal Escalation)
A banking portal exposes account balances via an API endpoint:
```http
GET /api/v1/accounts/10425/balance HTTP/1.1
Host: bank.example.com
Cookie: session=AliceSessionToken
```
If Bob logs in (with `session=BobSessionToken`) and alters the path to request `/api/v1/accounts/10425/balance`, a vulnerable server checks only that Bob has a valid session and returns Alice's financial balance because it failed to verify that account `10425` belongs to Bob.

### Example B: Vertical Privilege Escalation via Parameter Tampering
During profile update, an application submits:
```json
{
  "name": "Attacker",
  "email": "attacker@example.com",
  "role": "admin"
}
```
If the backend binds the incoming JSON payload directly to the user model without filtering out protected properties (mass assignment), the user elevates their privileges to administrator.

### Example C: Server-Side Request Forgery (SSRF as an Access Control Failure)
An application allows users to fetch their profile avatar from an external URL:
```http
POST /api/avatar/fetch HTTP/1.1
Content-Type: application/json

{"imageUrl": "http://169.254.169.254/latest/meta-data/iam/security-credentials/"}
```
The server makes the outbound request from its own internal IP address, bypassing cloud perimeter access controls to leak cloud IAM credentials.

---

## 4. Common Causes

- **Absence of Centralized Enforcement:** Access control decisions scattered across controllers, views, and raw SQL queries rather than enforced consistently via centralized middleware.
- **Relying on Obscurity:** Assuming unlinked administrative paths (e.g., `/admin-console-xyz123`) cannot be discovered or accessed.
- **Trusting Client-Supplied Identity:** Reading tenant or user IDs from query parameters, request bodies, or unverified JWT claims rather than securely bound server-side session state.
- **Method-Level Access Control Flaws:** Enforcing authentication on `GET` requests while allowing unauthenticated `POST`, `PUT`, or `OPTIONS` requests.
- **Permissive CORS Policies:** Returning `Access-Control-Allow-Origin: *` or reflecting arbitrary `Origin` headers with `Access-Control-Allow-Credentials: true`.

---

## 5. Potential Impact

- **Full Data Breach:** Mass unauthorized retrieval of PII, financial records, medical history, or confidential business data.
- **Unauthorized Data Modification or Destruction:** Deletion of audit logs, tampering with account balances, or altering configurations.
- **Full Account & Administrative Takeover:** Attackers seizing admin accounts, creating persistent backdoors, or compromising entire multi-tenant infrastructures.
- **Internal Network Exposure:** Through SSRF, accessing cloud metadata services, databases, Kubernetes APIs, and internal microservices.

---

## 6. How to Detect or Test It

### Manual Testing with Burp Suite
1. **Multi-Account Testing:** Configure two standard accounts (User A and User B) and one administrative account.
2. **Horizontal Checks:** Intercept User A's requests, replace session cookies with User B's session cookies, and check whether User B can view or modify User A's resources.
3. **Vertical Checks:** Attempt to replay administrative requests (e.g., `/admin/createUser`) using User A's low-privileged session token.
4. **Burp Suite Extensions:** Use extensions such as **Autorize** or **AuthMatrix** to automate cross-role access matrix verification.
5. **Method & Header Tampering:** Change `GET` requests to `POST`/`PUT`/`PATCH`, append unexpected headers (`X-Original-URL: /admin`), and test path manipulation (`/app/public/../admin`).

### Source Code Review
- Inspect controllers and routes to verify that authorization decorators/guards are universally applied.
- Verify database queries scope objects to the session user:
  ```sql
  -- Secure pattern:
  SELECT * FROM documents WHERE id = :doc_id AND owner_user_id = :session_user_id;
  ```

---

## 7. How to Prevent or Fix It

1. **Deny by Default:** Design systems so access is blocked unless explicitly granted by a rule.
2. **Centralize Authorization:** Implement access control policies in reusable middleware or a dedicated authorization layer (e.g., OPA, Casbin, Spring Security filters) rather than ad-hoc inline checks.
3. **Enforce Record Ownership Server-Side:** Never query resources solely by user-controlled IDs. Always cross-reference the requested ID with the authenticated session's user/tenant ID.
4. **Use Indirect Reference Maps or Unpredictable IDs:** Replace predictable auto-incrementing integer IDs with cryptographically random UUIDv4 identifiers, while maintaining server-side authorization checks.
5. **Disable Directory Browsing & Enforce Strict CORS:** Prevent web servers from listing directory contents and ensure CORS origins are validated against a strict allowlist without wildcards when credentials are supported.
6. **Log and Alert on Violations:** Immediately log access control check failures and alert security teams when abnormal patterns (e.g., sequential IDOR attempts) occur.

---

## 8. Official Learning Resources

- **OWASP Top 10:2025 Category:** [A01:2025 – Broken Access Control](https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/)
- **OWASP Cheat Sheet:** [OWASP Access Control Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html)
- **OWASP Testing Guide:** [OWASP Web Security Testing Guide: Authorization Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/)
- **PortSwigger Web Security Academy:**
  - [Access Control Vulnerabilities & Privilege Escalation](https://portswigger.net/web-security/access-control)
  - [Insecure Direct Object References (IDOR)](https://portswigger.net/web-security/access-control/idor)
  - [Server-Side Request Forgery (SSRF)](https://portswigger.net/web-security/ssrf)
  - [Cross-Origin Resource Sharing (CORS)](https://portswigger.net/web-security/cors)
