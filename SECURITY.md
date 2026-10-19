# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.x     | ✅ Yes    |

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

Email: security@your-domain.com (replace with your address)

Please include:
1. A description of the vulnerability
2. Steps to reproduce
3. Potential impact
4. Any suggested mitigations

We will acknowledge receipt within 48 hours and aim to release a fix within 14 days for critical issues.

## Scope

In scope:
- Signature verification bypass
- Webhook SSRF (Server-Side Request Forgery)
- Sensitive data leakage in logs or payloads
- Local database exposure
- Permission escalation

Out of scope:
- Issues requiring physical access to an already-compromised device
- Social engineering attacks on the merchant

## Security Architecture

See [docs/threat-model.md](docs/threat-model.md) for full threat analysis.

Key controls:
- All webhook payloads are HMAC-SHA256 signed
- Timestamps and event IDs prevent replay attacks
- Only HTTPS endpoints accepted; private IP ranges blocked
- No data leaves the device except to the merchant's configured endpoint
- Android Keystore protects signing secrets at rest
- SMS text is never included in webhook payloads
