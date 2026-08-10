# Scope

## Target

- **Primary domain:** `https://www.bharat-evote.me`
- **In-scope endpoints tested:**
  - `https://www.bharat-evote.me/` (main application)
  - `https://www.bharat-evote.me/admin-login`
  - File upload functionality (voter/record CSV upload)
  - Public-facing HTTP response headers / server configuration
  - `robots.txt` and related metadata files

## In Scope

- Web application layer testing (OWASP Top 10 2025 categories)
- Authentication and session management (login, CSRF, cookies/tokens)
- Server security header / CORS configuration review
- File upload handling and validation
- Client-side script/resource integrity (SRI, CSP)
- Passive reconnaissance and technology fingerprinting (e.g., via
  Wappalyzer) for informational purposes

## Out of Scope

- Denial-of-Service (DoS/DDoS) testing of any kind
- Social engineering, phishing, or physical security testing
- Testing against real voter/election data or any production election event
- Any third-party services, subdomains, or infrastructure not directly
  operated by the bharat-evote.me application (e.g., hosting provider
  infrastructure, DNS registrar, CDN backend)
- Automated mass-scanning / brute-force attacks against login endpoints
- Any destructive testing (data deletion, data modification beyond what is
  necessary to demonstrate a PoC in a non-destructive way)
- Source code review (black-box/grey-box testing only, unless otherwise
  noted)

## Rules of Engagement

- Testing was performed with the knowledge and authorization of the site
  owner.
- Any testing that could impact availability or integrity of live data was
  avoided or coordinated with the owner beforehand.
- All findings were reported directly to the owner prior to any public
  documentation.
