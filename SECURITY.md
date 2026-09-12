# Security Policy — FLATLINED Organization

Security is fundamental to our mission. We take security vulnerabilities seriously and appreciate the efforts of security researchers and practitioners who practice responsible disclosure.

## Scope

This security policy applies to all active repositories under the [FLATLINED](https://github.com/FLATLINEDSTAR) organization.

## Supported Versions

Only the latest commit on the default branch (`main`) of active repositories is officially supported for security fixes. Given the fast-evolving nature of pre-1.0 projects, patches are not backported to older commits or untagged states.

| Project | Branch | Supported |
| :--- | :--- | :--- |
| `OnionScan` (OnionSec) | `main` | Yes |
| All other repositories | `main` | Yes |

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

To report a vulnerability:

1. Use **GitHub Private Vulnerability Reporting** directly on the affected repository:
   - Navigate to the repository on GitHub.
   - Click the **Security** tab.
   - Click **Report a vulnerability** under "Advisories".
2. If private vulnerability reporting is unavailable for any reason, reach out directly to the organization maintainers via repository security advisories.

### What to Include

When reporting a vulnerability, please provide:

- A clear description of the vulnerability and its potential impact.
- Step-by-step instructions or minimal reproducible proof-of-concept (POC) code.
- Affected component, module, CLI command, or API endpoint.
- Any suggested mitigations or patches if available.

### What NOT to Disclose Publicly

- Exploit scripts or weaponized payloads.
- Detailed reproduction steps before a fix has been released.
- Personally identifiable information (PII) or secrets found during research.

## Response Process

Upon receipt of a vulnerability report:

1. **Acknowledgment:** We will acknowledge receipt of the report within 48 hours.
2. **Investigation & Triage:** We will assess the severity and impact within 5 business days.
3. **Remediation:** We will develop, review, and test a fix in a private repository fork or security advisory workspace.
4. **Release & Disclosure:** We will publish a patched release along with a coordinated security advisory crediting the reporter (unless anonymity is requested).
