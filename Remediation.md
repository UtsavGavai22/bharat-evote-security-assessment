# Remediation

Recommended fixes for each finding in `Findings.md`, ordered by priority.

---

## 1. Cleartext Transmission of Credentials

- Enforce HTTPS/TLS across the entire application with HSTS
  (`Strict-Transport-Security` header) enabled.
- Ensure no fallback to HTTP exists for any authentication endpoint.
- Avoid logging or storing raw credentials anywhere in application logs.
- Consider implementing MFA for admin accounts as defense-in-depth.

## 2. CORS Misconfiguration

- Replace `Access-Control-Allow-Origin: *` with an explicit allowlist of
  trusted origins that require cross-origin access.
- Never combine a wildcard origin with `Access-Control-Allow-Credentials:
  true`.
- Review `robots.txt` and ensure it does not unintentionally expose or
  invite crawling of sensitive paths.

## 3. Missing Content-Security-Policy (CSP) Header

- Configure the web server / application to send a
  `Content-Security-Policy` header on every response.
- Start with a restrictive baseline policy (e.g., `default-src 'self'`) and
  incrementally allow only the specific external sources actually required
  (fonts, analytics, CDNs, etc.).
- Add `frame-ancestors` and `script-src` directives to also address the
  clickjacking and SRI findings above as part of the same policy.
- Test the policy in `Content-Security-Policy-Report-Only` mode first to
  catch breakage before enforcing it.
- Reference: https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html

## 4. Missing Anti-Clickjacking Protection

- Add the `X-Frame-Options: DENY` (or `SAMEORIGIN` if framing is required
  internally) header to all responses.
- Additionally implement `Content-Security-Policy: frame-ancestors 'none'`
  (or `'self'`) for modern browser support.

## 5. Missing Subresource Integrity (SRI)

- Add `integrity` and `crossorigin` attributes to all `<script>`/`<link>`
  tags loading third-party resources.
- Where feasible, self-host critical third-party libraries instead of
  loading from external CDNs.
- Implement a strict `Content-Security-Policy` to restrict which origins
  scripts can be loaded from, using `nonce`s for inline scripts.

## 6. File Upload Validation Bypass

**Priority: Critical — address immediately.**

- Validate file type server-side using file content/magic-byte inspection,
  not just filename extension.
- Store uploaded files outside the webroot, or in a location where the web
  server is configured to never execute scripts (e.g., disable script
  execution in the upload directory).
- Rename uploaded files server-side to a generated name with a fixed,
  whitelisted extension.
- Run uploaded file processing in a sandboxed environment where possible.
- Set correct `Content-Type` headers when serving uploaded files, and serve
  them from a separate domain/subdomain with no execution privileges if
  possible.

## 7. Missing CSRF Protection

- Implement anti-CSRF tokens (synchronizer token pattern) on all
  state-changing requests, validated server-side on every request.
- Set session cookies with `SameSite=Strict` or `SameSite=Lax` as
  appropriate.
- Ensure server-side session/authorization checks are enforced
  independently of client-supplied tokens — a missing or invalid CSRF token
  should always result in request rejection.

## 8. Technology Fingerprinting Exposure

- Remove or obscure unnecessary version-revealing headers (e.g., `Server`,
  `X-Powered-By`).
- Keep all frameworks and libraries patched and up to date to minimize the
  impact of any fingerprinting that does occur.

---

## General Recommendations

- Conduct regular security assessments, especially before and after major
  releases or election-related events.
- Adopt a Content-Security-Policy across the whole application as a
  broad mitigation for several of the above findings simultaneously.
- Consider a formal responsible-disclosure / bug bounty policy to
  encourage ongoing reporting of issues by researchers.
