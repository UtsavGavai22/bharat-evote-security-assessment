# Tools Used

| Tool | Purpose |
|------|---------|
| **Nmap** | Network/service reconnaissance and port scanning |
| **Burp Suite** | Intercepting proxy — request/response inspection and manipulation (CSRF, header, auth testing) |
| **OWASP ZAP (ZAProxy)** | Automated and manual web application vulnerability scanning |
| **Wireshark** | Network traffic capture and analysis (verifying cleartext transmission) |
| **Wappalyzer** | Passive technology stack fingerprinting during reconnaissance |
| **IBM AppScan** | Automated application security scanning |
| **Browser DevTools** (Chrome/Firefox) | Inspecting request/response traffic, headers, cookies, and client-side behavior |
| **Manual request manipulation** | Testing CSRF protections by modifying/removing cookies and tokens |
| **curl** | Verifying response headers (CORS, CSP, X-Frame-Options, etc.) |
| ...and other supporting utilities as needed during the engagement | |

## Notes

- A combination of **manual testing** and **automated scanning tools** was
  used, with automated scans (Nmap, ZAP, AppScan) run only after
  coordination with the site owner and scoped to avoid impact on
  availability or production data.
- Burp Suite and Wireshark were used primarily for traffic inspection and
  validating findings (e.g., confirming cleartext credential transmission)
  rather than active exploitation.
- Manual verification was prioritized for confirming exploitability and
  reducing false positives from automated tool output.
- Tool versions and exact configurations used are available on request as
  part of the private engagement report.
