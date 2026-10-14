# A03:2025 – Software Supply Chain Failures

## 1. What the Vulnerability Is
Software Supply Chain Failures is a major category in OWASP Top 10:2025 that broadens previous focus on vulnerable components to cover risks across the entire software ecosystem. It includes vulnerabilities in third-party libraries, malicious packages (typosquatting, dependency confusion), compromised CI/CD pipelines, untrusted build plugins, and unverified distribution channels.

## 2. How It Works
Modern software is built using hundreds or thousands of open-source libraries and build tools. Attackers exploit known public vulnerabilities (CVEs) in outdated dependencies, compromise upstream developer accounts, publish malicious packages with names similar to popular libraries, or tamper with CI/CD build pipelines to inject backdoors directly into release artifacts.

## 3. Simple Illustrative Example
A Node.js application imports a widely-used helper package to merge JavaScript objects. The library contains an unpatched prototype pollution flaw. An attacker submits a malicious JSON payload:
```json
{
  "__proto__": {
    "isAdmin": true
  }
}
```
Because the application recursively merges untrusted input into the global Object prototype, all user objects in the runtime inherit `isAdmin = true`, bypassing authorization checks globally.

## 4. Common Causes
- Neglecting to track, inventory, and update third-party libraries and nested dependencies.
- Lack of a Software Bill of Materials (SBOM) to monitor deployed components.
- Insecure dependency resolution rules allowing public package repositories to take precedence over internal packages (dependency confusion).
- Unpinned dependency versions in build manifests (e.g., using `latest` or loose version ranges `^` instead of exact lockfiles).
- Compromised or unvetted continuous integration (CI) runners and third-party build plugins.

## 5. Potential Impact
- Remote Code Execution (RCE) on production servers via unpatched library flaws.
- Infiltration of crypto-miners, data stealers, or backdoors directly into consumer-facing binaries.
- Compromise of production signing keys and build infrastructure.

## 6. How to Detect or Test It
- Software Composition Analysis (SCA) tooling: `npm audit`, `pip-audit`, OWASP Dependency-Check, Snyk, or Trivy.
- Continuous vulnerability scanning integrated into CI/CD pipelines to fail builds upon detecting high/critical CVEs.
- Verifying cryptographic hashes of downloaded packages against official lockfiles (`package-lock.json`, `poetry.lock`).

## 7. How to Prevent or Fix It
- Maintain an accurate, up-to-date Software Bill of Materials (SBOM) using standards like CycloneDX or SPDX.
- Pin exact dependency versions and enforce lockfile integrity verification.
- Use internal artifact proxy repositories (e.g., Nexus, Artifactory) with vulnerability scanning and namespace scoping to prevent dependency confusion.
- Subscribe to security advisories and automate dependency updates using tools like Dependabot or Renovate.
- Sign and verify software artifacts and container images using tools like Sigstore/Cosign.

## 8. Official Learning Resources
- **OWASP Top 10:2025:** [A03:2025 – Software Supply Chain Failures](https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/)
- **OWASP Project:** [OWASP Software Component Verification Standard (SCVS)](https://owasp.org/www-project-software-component-verification-standard/)
- **OWASP Project:** [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)
- **PortSwigger Web Security Academy:**
  - [Server-side Prototype Pollution in Third-Party Libraries](https://portswigger.net/web-security/prototype-pollution/server-side)
  - [Client-side Prototype Pollution](https://portswigger.net/web-security/prototype-pollution)
