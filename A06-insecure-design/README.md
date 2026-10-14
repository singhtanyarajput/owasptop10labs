# A06:2025 – Insecure Design

## 1. What the Vulnerability Is
Insecure Design focuses on risks and architectural flaws resulting from missing or ineffective security design, lack of threat modeling, and flawed business logic. Unlike implementation bugs (which can be resolved by patching a line of code), insecure design represents conceptual defects where the application behaves exactly as built, but the underlying architecture fails to defend against realistic threats.

## 2. How It Works
Insecure design happens before any code is written. When developers fail to anticipate attacker behaviors, omit threat modeling, assume user actions will always occur in an ideal sequence, or omit critical defensive controls (like rate limits, balance validation, or multi-step transaction safeguards), attackers exploit the legitimate logic to achieve malicious goals.

## 3. Simple Illustrative Example
An e-commerce site provides a coupon code for $20 off an order. The application decrements the coupon validity *after* fulfilling the order. An attacker sends 15 concurrent HTTP requests to checkout using Burp Suite Turbo Intruder in a race condition:
```http
POST /cart/checkout HTTP/1.1
Content-Type: application/json

{"coupon": "PROMO20", "cartId": "482"}
```
Because the system lacks thread locking or atomic transaction design, all 15 concurrent requests read the coupon as valid before any write locks take effect, applying a $300 discount to an unauthorized order.

## 4. Common Causes
- Omission of security architecture reviews and formal threat modeling (e.g., STRIDE) during design phases.
- Assuming client-side controls, workflows, or validation cannot be circumvented.
- Flawed business logic assumptions (e.g., negative item quantities, unverified workflow sequences).
- Failure to implement rate limiting or anti-automation controls on business-critical transactions.
- Missing separation of duties and defense-in-depth safeguards.

## 5. Potential Impact
- Direct financial loss via coupon abuse, price tampering, or unauthorized credit balance manipulation.
- Bypass of critical business processes (e.g., purchasing without payment confirmation).
- Mass account harvesting or enumeration due to flawed recovery workflows.

## 6. How to Detect or Test It
- Architectural threat modeling sessions during sprint planning and system design.
- Manual business logic testing: manipulating request sequences, skipping intermediate steps, testing boundary values, and sending negative numbers.
- Concurrency and race condition testing using tools like Burp Suite Repeater (Send in parallel) or Turbo Intruder.

## 7. How to Prevent or Fix It
- Integrate threat modeling into the software development lifecycle (SDLC) before development starts.
- Enforce strict server-side state machines to ensure users complete workflow steps in the correct order.
- Design database operations to be atomic and thread-safe using transactions and database-level constraints.
- Implement defense-in-depth: never rely on a single defensive layer to protect critical assets.
- Validate all business assumptions (e.g., positive quantities, valid price bounds) on the backend.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A06:2025 – Insecure Design](https://top10.owasp.org/2025/A06_2025-Insecure_Design/)
- **OWASP Project:** [OWASP Threat Modeling](https://owasp.org/www-community/Threat_Modeling)
- **PortSwigger Web Security Academy:**
  - [Business Logic Vulnerabilities](https://portswigger.net/web-security/logic-flaws)
  - [Race Conditions](https://portswigger.net/web-security/race-conditions)
