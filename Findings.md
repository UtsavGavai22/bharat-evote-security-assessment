# Findings

> **Note on disclosure level:** These findings are written for a public
> audience following responsible disclosure practice. Each entry describes
> the vulnerability class, where it was observed, its impact, and how it was
> verified — without including exact working exploit payloads. Full
> proof-of-concept detail was provided to the site owner directly as part of
> the private engagement report.

---

## 1. Cleartext Transmission of Credentials

**OWASP Category:** A04:2025 – Cryptographic Failures
**Endpoint:** `/admin-login`
**Severity:** High

### Description
Login requests to the admin panel transmit the `username` and `password`
parameters in plain text within the request body, observable via browser
DevTools / an intercepting proxy. If the connection is not properly
enforced over TLS end-to-end, or if any intermediary can observe traffic,
credentials are exposed.

### Impact
An attacker positioned to observe network traffic (e.g., on shared/insecure
networks, or via a misconfigured TLS setup) could capture admin
credentials, leading to full account/administrative compromise.

### Evidence
Captured via browser DevTools Network tab during a legitimate login attempt
(see `screenshots/`, credentials redacted).

---

## 2. CORS Misconfiguration (Wildcard Origin)

**OWASP Category:** A05:2025 – Security Misconfiguration
**Endpoint:** Site-wide (`https://www.bharat-evote.me`)
**Severity:** Medium–High

### Description
The web server responds with an `Access-Control-Allow-Origin: *` header,
permitting cross-origin requests from **any** origin. Additionally,
`robots.txt` is configured in a way that does not restrict crawling/access
by origin, compounding exposure of the application's structure.

### Impact
A permissive CORS policy can allow a malicious site to make authenticated
cross-origin requests on behalf of a victim (if combined with cookie-based
auth) and read the responses, potentially leaking sensitive data returned
by authenticated API endpoints.

### Evidence
Confirmed via response headers inspected in browser DevTools / proxy tool.
Additional instances of the missing `Access-Control-Allow-Origin`
restriction (returning `*`) were observed via automated scan across
multiple endpoints, including `/`, `/admin-login`, and `robots.txt`.

---

## 3. Missing Content-Security-Policy (CSP) Header

**OWASP Category:** A05:2025 – Security Misconfiguration
**Endpoint:** Site-wide — confirmed on `/`, `/admin-login`, `/admin-panel`,
`/sitemap.xml`, `/vote` (5 instances observed)
**Severity:** Medium

### Description
The application does not set a `Content-Security-Policy` HTTP header on
any of the tested pages. CSP is a browser-enforced allowlist mechanism
that restricts which sources of scripts, styles, frames, fonts, images,
and other embeddable content a page is permitted to load. Without it, the
browser has no additional layer of defense against injected content if an
XSS or data-injection flaw is later found (or already exists) elsewhere in
the application.

### Impact
In the absence of CSP, any Cross-Site Scripting (XSS) vulnerability found
in the application would be significantly easier to exploit, since there
is no browser-level restriction preventing an injected script from
running, exfiltrating data, or loading external malicious resources. CSP
would also provide defense-in-depth against several of the other findings
in this report (clickjacking, missing SRI).

### Evidence
Confirmed via automated scan (OWASP ZAP) and manual verification of
response headers across the endpoints listed above. No
`Content-Security-Policy` header was present in any response.

### References
- CWE-693: Protection Mechanism Failure
- OWASP CSP Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html

---

## 4. Missing Anti-Clickjacking Protection

**OWASP Category:** A05:2025 – Security Misconfiguration
**Endpoint:** Site-wide
**Severity:** Medium

### Description
Application responses do not include `X-Frame-Options` or a
`Content-Security-Policy` with a `frame-ancestors` directive. This means the
site can be embedded in an `<iframe>` on an attacker-controlled page.

### Impact
Enables clickjacking attacks, where an attacker overlays invisible UI
elements over the framed application to trick users (including
administrators) into performing unintended actions (e.g., a disguised
login or vote-related action).

### Evidence
Confirmed by successfully framing the target page in a local test HTML
page and inspecting response headers.

---

## 5. Missing Subresource Integrity (SRI) on Externally-Loaded Scripts

**OWASP Category:** A03:2025 – Software Supply Chain Failures
**Endpoint:** Site-wide (script/link tags referencing external resources)
**Severity:** Medium

### Description
Script and link tags that load resources from third-party/external
domains do not include the `integrity` attribute (Subresource Integrity).
Without SRI, if the external host is compromised or the resource is
tampered with in transit, the browser will load and execute the modified
content without any validation.

### Impact
If an attacker gains control of an external resource host, they could
inject malicious JavaScript that executes in the context of the
application, potentially leading to session hijacking, credential theft,
or defacement.

### Recommended Alternatives
Beyond adding `integrity` attributes, comparable protection can be achieved
by:
1. Self-hosting third-party resources instead of loading them externally.
2. Enforcing a strict `Content-Security-Policy` header.
3. Using CSP `nonce`-based script allowlisting.

---

## 6. File Upload Extension Validation Bypass

**OWASP Category:** A03:2025 – Injection / Software & Data Integrity Failures
**Endpoint:** File upload functionality
**Severity:** Critical

### Description
The file upload feature is intended to accept only `.csv` files. Testing
found that extension validation is performed client-side / via a weak
server-side check that can be bypassed by manipulating the filename, which
resulted in a non-CSV file being accepted and stored in a location where it
could be executed by the server.

### Impact
This represents a potential path to **remote code execution** if an
uploaded file can be both stored and subsequently invoked by the web
server. This is the most severe finding in this assessment and was flagged
to the site owner as top priority.

### Evidence
Verified by observing that a specially-crafted filename was accepted by the
upload form despite the intended `.csv`-only restriction. Exact payload
and reproduction steps were provided privately to the site owner and are
withheld from this public writeup.

---

## 7. Missing CSRF Protection

**OWASP Category:** A01:2025 – Broken Access Control
**Endpoint:** Authenticated/admin actions
**Severity:** High

### Description
State-changing requests do not appear to be protected by an anti-CSRF
token. Testing showed that removing the session cookie and authentication
token from a request did not prevent the request from completing
successfully, suggesting authorization/session validation is not
consistently enforced server-side for these actions.

### Impact
Combined with the CORS and clickjacking issues above, this significantly
raises the risk of CSRF attacks — tricking an authenticated user (including
an admin) into unknowingly performing privileged actions.

### Evidence
Verified via manual request manipulation (removing cookie/token headers)
using browser DevTools and observing the response.

---

## 8. Information Disclosure via Technology Fingerprinting

**OWASP Category:** A05:2025 – Security Misconfiguration
**Endpoint:** Site-wide
**Severity:** Low–Informational

### Description
Using the Wappalyzer browser extension during the reconnaissance phase, it
was possible to identify the underlying frameworks, libraries, and server
technologies in use by the application.

### Impact
While not directly exploitable on its own, technology fingerprinting
lowers the effort required for an attacker to identify known
vulnerabilities associated with specific software versions in use.

### Evidence
Screenshot of Wappalyzer output (see `screenshots/`).

---

## Summary Table

| # | Finding | OWASP Category | Severity |
|---|---------|-----------------|----------|
| 1 | Cleartext credential transmission | A04:2025 Cryptographic Failures | High |
| 2 | CORS misconfiguration (wildcard) | A05:2025 Security Misconfiguration | High |
| 3 | Missing Content-Security-Policy header | A05:2025 Security Misconfiguration | Medium |
| 4 | Missing anti-clickjacking headers | A05:2025 Security Misconfiguration | Medium |
| 5 | Missing SRI on external scripts | A03:2025 Software Supply Chain Failures | Medium |
| 6 | File upload validation bypass | A03:2025 Software/Data Integrity Failures | Critical |
| 7 | Missing CSRF protection | A01:2025 Broken Access Control | High |
| 8 | Technology fingerprinting exposure | A05:2025 Security Misconfiguration | Low |
