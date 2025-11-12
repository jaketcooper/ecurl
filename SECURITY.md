# Security Policy

## Supported Versions

The following versions of `ecurl` receive security updates and maintenance:

| Version | Supported |
|----------|------------|
| 1.0.x | ✅ Yes |
| < 1.0 | ❌ No |

---

## Reporting a Vulnerability

If you discover a security vulnerability in `ecurl` or its packaging:

1. **Do not** publicly disclose the issue until it has been confirmed and patched.
2. Contact the maintainer directly at:

   **security@jaketcooper.dev**  
   *(or use GitHub's private vulnerability disclosure feature if available).*

3. Include:
   - A clear, reproducible description of the issue
   - Your operating environment and `ecurl --version`
   - Proof-of-concept commands or payloads if applicable
   - Suggested mitigations if known

---

## Handling Procedure

- Vulnerability reports are acknowledged within **72 hours**.
- A fix or mitigation will be developed, tested, and pushed to the `dev` branch first.
- Responsible disclosure credit will be granted in the changelog unless anonymity is requested.

---

## Disclosure Policy

All vulnerabilities are disclosed publicly **after a patch is available**.  
Critical issues may trigger a **CVE request** if criteria are met under Debian or Kali’s security policy.

| Severity | Action |
|-----------|--------|
| **Critical** (RCE, privilege escalation) | CVE request + immediate patch |
| **High** (data exposure, DoS) | 72-hour response window |
| **Medium** (information leak, config flaw) | Next minor release |
| **Low** (non-exploitable) | Logged, tracked for next maintenance cycle |

---

## Security Best Practices

To minimize risk:
- Run `ecurl` only in controlled or authorized environments.
- Keep dependency packages (`curl`, `perl`, `jq`) updated.
- Use isolated API keys or test accounts for payload work.
- Avoid saving sensitive targets in persistent history (`~/.ecurl_history`).
- Review and sanitize tamper scripts before use.

---

## Responsible Usage Reminder

`ecurl` is a tool for **ethical security testing**.  
Misuse against unauthorized targets is illegal and unethical.  
Users are expected to comply with all applicable laws, contracts, and NDAs.

---

**Maintainer:** Jake Cooper  
**Email:** security@jaketcooper.dev  
**License:** MIT  
**Last Updated:** November 12, 2025
