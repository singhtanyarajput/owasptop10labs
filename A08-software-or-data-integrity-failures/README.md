# A08:2025 – Software or Data Integrity Failures

## 1. What the Vulnerability Is
Software or Data Integrity Failures occurs when software code, application state, or critical data is accepted and processed from untrusted sources without verifying its integrity and authenticity. This category includes insecure deserialization, unverified auto-update mechanisms, unsigned code deployment, and unvalidated data serialization pipelines.

## 2. How It Works
Applications serialize complex objects into binary, JSON, XML, or base64 streams to transmit them over networks or persist them in cookies and caches. When the application reconstructs (deserializes) these objects without integrity checks (like cryptographic signatures), an attacker can tamper with object fields or craft a malicious "gadget chain" that executes arbitrary code during object instantiation.

## 3. Simple Illustrative Example
A Python web application stores user session information in a cookie using the `pickle` serialization module:
```http
GET /dashboard HTTP/1.1
Cookie: session=gASVNAAAAAAAAACMC... (base64 pickled object)
```
An attacker crafts a custom pickled payload with an embedded `__reduce__` method calling `os.system('id')`. When the server calls `pickle.loads(cookie)`, the payload executes arbitrary system commands with the privileges of the web application.

## 4. Common Causes
- Deserializing untrusted data streams using native object deserializers (Java `ObjectInputStream`, Python `pickle`, PHP `unserialize()`, .NET `BinaryFormatter`).
- Auto-update functionalities that download binaries over unencrypted channels or without verifying digital signatures.
- Trusting client-side state objects or signed tokens (e.g., JWT) without verifying the signature key or allowing `none` algorithm.
- Importing external client scripts from third-party CDNs without Subresource Integrity (SRI) hashes.

## 5. Potential Impact
- Arbitrary Remote Code Execution (RCE) on backend application servers.
- Privilege escalation by modifying serialized permission objects.
- Injection of malicious scripts or trojans into user browsers via tampered CDN scripts.

## 6. How to Detect or Test It
- Identify serialized object signatures in HTTP parameters and cookies (e.g., Java serialized objects starting with `rO0AB` or hex `ac ed 00 05`, PHP serialized strings starting with `O:`, Python pickle headers).
- Test endpoints using payload generation tools like `ysoserial` or `ysoserial.net` in safe test environments.
- Static analysis scanning for unsafe deserialization functions and missing signature verifications.

## 7. How to Prevent or Fix It
- Avoid native object serialization formats. Use pure data-interchange formats like JSON, Protocol Buffers, or XML with strict schema validation.
- If serialization of complex objects is required, apply cryptographic signatures (HMAC) to the serialized payload and verify the signature before deserializing.
- Enforce Subresource Integrity (SRI) attributes on all external `<script>` and `<link>` tags.
- Ensure software updates, dependencies, and plugins are cryptographically signed and verified against trusted public keys before installation.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A08:2025 – Software or Data Integrity Failures](https://top10.owasp.org/2025/A08_2025-Software_or_Data_Integrity_Failures/)
- **OWASP Cheat Sheet:** [Deserialization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html)
- **PortSwigger Web Security Academy:**
  - [Insecure Deserialization](https://portswigger.net/web-security/deserialization)
  - [DOM-based Vulnerabilities](https://portswigger.net/web-security/dom-based)
