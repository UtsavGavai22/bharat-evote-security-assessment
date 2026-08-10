# Testing Methodology

This assessment followed a structured, industry-standard web application
penetration testing methodology, broadly aligned with the **OWASP Testing
Guide** and **OWASP Top 10 (2025)** categories.

## 1. Reconnaissance & Information Gathering

- Passive and light active reconnaissance of the target domain.
- Technology stack fingerprinting using browser DevTools and Wappalyzer to
  identify frameworks, server software, and third-party libraries.
- Review of publicly accessible metadata files (`robots.txt`, response
  headers) for configuration leakage.

## 2. Configuration & Deployment Management Testing

- Review of HTTP response headers for missing or misconfigured security
  headers (e.g., `Content-Security-Policy`, `X-Frame-Options`).
- Review of CORS (Cross-Origin Resource Sharing) configuration, checking
  `Access-Control-Allow-Origin` and related headers against expected
  same-origin policy.

## 3. Authentication & Session Management Testing

- Manual inspection of login request/response traffic using browser
  DevTools to check whether credentials are transmitted securely.
- Review of session handling: cookies, tokens, and their attributes
  (`HttpOnly`, `Secure`, `SameSite`).
- Testing for Cross-Site Request Forgery (CSRF) protections by
  manipulating/removing session cookies and authentication tokens and
  observing whether privileged requests still succeed.

## 4. Input Validation & File Upload Testing

- Testing of file upload functionality for extension/type validation
  bypasses.
- Verification of server-side handling of uploaded file types beyond the
  officially supported format.

## 5. Client-Side Security Testing

- Review of externally-loaded scripts/resources for Subresource Integrity
  (SRI) attributes.
- Assessment of clickjacking protections (frame-busting headers /
  `X-Frame-Options` / `frame-ancestors`).

## 6. Analysis & Reporting

- Each finding was mapped to the relevant **OWASP Top 10 2025** category.
- Findings were assessed for real-world impact and exploitability.
- Findings were reported to the site owner prior to any public
  documentation, following a responsible disclosure approach.

## Approach

- **Testing type:** Black-box / grey-box, manual testing (primary), with
  supporting tooling for fingerprinting and traffic inspection.
- **Automated scanning:** Minimal — used only for fingerprinting
  (Wappalyzer). No automated vulnerability scanners or brute-force tools
  were run against the live application without prior coordination.
- **Evidence collection:** Screenshots and request/response captures were
  taken for each finding; sensitive data (credentials, tokens, personal
  data) was redacted before inclusion in any report.
