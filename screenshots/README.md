# Screenshots

This folder contains **non-sensitive** supporting evidence referenced in
`Findings.md`.

## Guidelines before adding a screenshot here

- ❌ No real credentials, passwords, or session tokens (even partially
  visible)
- ❌ No real voter/personal data
- ❌ No full working exploit payloads or step-by-step reproduction that
  would let a third party replicate the attack today
- ✅ Redact/blur any sensitive values before adding an image
- ✅ Prefer cropped, annotated screenshots that illustrate the *finding*
  (e.g., a response header) rather than a full page capture

## Current contents

| File | Finding | Notes |
|------|---------|-------|
| `finding-00-recon-nmap-open-ports.png` | Recon | Nmap scan showing exposed non-essential services |
| `finding-02-cors-misconfiguration.png` | #2 CORS misconfiguration | Credentials in request body were redacted before upload |
| `finding-02b-robots-txt-disclosure.png` | #2 (supporting) | robots.txt revealing admin/vote paths |
| `finding-04-clickjacking-framed-site.png` | #4 Missing anti-clickjacking protection | Live site rendered inside attacker-controlled iframe |
| `finding-04-clickjacking-poc-payload.png` | #4 (supporting) | PoC exploit-server payload used to frame the site |
| `finding-06-file-upload-bypass-REDACTED.png` | #6 File upload validation bypass | Malicious payload contents fully redacted — only the extension-bypass evidence (`filename="test.php.csv"`, HTTP 200) is shown |
| `finding-08-tech-fingerprint-wappalyzer.png` | #8 Technology fingerprinting | Wappalyzer stack disclosure |
| `supporting-tls-configuration-testssl.png` | Methodology | testssl.sh output, included as supporting evidence of TLS configuration testing (no vulnerability found here) |

**Note:** Two source screenshots contained live sensitive data (plaintext
login credentials, and a working PHP webshell payload with a partial
session token) and were redacted before being added here. Full
unredacted evidence was shared with the site owner directly and is not
part of this public repository.
