# Darkly

> **42 School — Web Security Project**
> *by [cbopp](https://github.com/LilBoooopp) & [ilyanar](https://github.com/IlYAN-FISHERMAN)*

Darkly is a hands-on web penetration testing project from the 42 curriculum. The target is **BornToSec** — a deliberately vulnerable web application deployed as a local VM. The objective: discover and exploit 14 distinct security vulnerabilities, capture a flag for each one, and document the full attack chain alongside remediation guidance.

No automated scanner solves this. Each vulnerability requires manual enumeration, understanding of the underlying flaw, and a crafted exploit.

---

## Vulnerabilities

| # | Vulnerability | OWASP Category |
|---|---------------|----------------|
| 1 | [Cookie Tampering — MD5 `I_am_admin` flag](#) | A07 — Identification & Authentication Failures |
| 2 | [Hidden Directory Enumeration via `robots.txt`](#) | A05 — Security Misconfiguration |
| 3 | [Hidden Page Access via HTTP Referer Spoofing](#) | A05 — Security Misconfiguration |
| 4 | [Exposed `.htpasswd` File Leaking Admin Credentials](#) | A05 — Security Misconfiguration |
| 5 | [SQL Injection — Member Search (`UNION SELECT`)](#) | A03 — Injection |
| 6 | [SQL Injection — Image Search + MD5/SHA256 Chain](#) | A03 — Injection |
| 7 | [Open Redirect — Unvalidated `site` Parameter](#) | A01 — Broken Access Control |
| 8 | [Path Traversal — Arbitrary File Read via `page`](#) | A01 — Broken Access Control |
| 9 | [Reflected XSS — Script Injection via `<object>` Tag](#) | A03 — Injection |
| 10 | [Unrestricted File Upload — PHP Webshell via Extension Bypass](#) | A04 — Insecure Design |
| 11 | [Login Brute Force — No Rate Limiting or Lockout](#) | A07 — Identification & Authentication Failures |
| 12 | [Password Recovery — Email Parameter Tampering](#) | A07 — Identification & Authentication Failures |
| 13 | [Survey Grade — Client-Side Maximum Value Bypass](#) | A04 — Insecure Design |
| 14 | [Feedback Form — Missing Server-Side Length Validation](#) | A04 — Insecure Design |

Each vulnerability lives in its own directory with:
- `flag` — the captured flag (SHA256 hash)
- `Resources/README.md` — full write-up: attack steps, root cause analysis, and remediation

---

## Attack Techniques Covered

- **SQL injection** — `UNION SELECT` enumeration via `information_schema`, multi-step hash chains (MD5 → SHA256)
- **XSS** — reflected injection through `<object data=>` with Base64 data URIs
- **Authentication bypass** — MD5 cookie forgery, HTTP header spoofing (`Referer`, `X-Forwarded-For`)
- **Credential attacks** — dictionary brute-force against an unprotected login form (Python script)
- **File upload exploitation** — MIME-type and extension bypass to upload a PHP webshell
- **Path traversal** — reading arbitrary files via a poorly sanitized `page` GET parameter
- **Open redirect** — abusing an unvalidated redirect parameter to point users off-site
- **Reconnaissance** — `robots.txt` enumeration, DirBuster directory scanning, `.htpasswd` exposure
- **Client-side trust abuse** — bypassing front-end `maxlength` and `<select>` constraints

---

## Methodology

```
Reconnaissance → Enumeration → Vulnerability Identification → Exploitation → Documentation
```

1. **Recon**: DirBuster scan, `robots.txt` review, HTML source inspection, cookie analysis
2. **Enumeration**: `sqlmap` for schema discovery, manual `UNION SELECT` chains, credential harvesting from exposed files
3. **Exploitation**: Crafted payloads per vulnerability — no automated exploit frameworks
4. **Documentation**: Each finding includes OWASP mapping, root cause table, and a concrete remediation with secure code examples

---

## Repository Structure

```
darkly/
├── admin_cookie/
│   ├── flag
│   └── Resources/README.md
├── members_injection/
│   ├── flag
│   └── Resources/README.md
├── password_brute_force/
│   ├── flag
│   └── Resources/
│       ├── brute_force.py
│       └── README.md
├── crawler/
│   ├── flag
│   └── Resources/
│       ├── crawler.sh
│       └── README.md
├── [11 more vulnerability directories...]
└── README.md
```

---

## Skills Demonstrated

- Web application security testing (black-box, manual)
- SQL injection — enumeration and data exfiltration
- XSS identification and exploitation
- Authentication and session management flaws
- Secure coding — parameterized queries, HMAC signing, server-side validation
- OWASP Top 10 (2021) applied in practice
- Python scripting for automated attack tooling
- Technical write-up and remediation documentation
