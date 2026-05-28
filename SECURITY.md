# Security Policy

Patchbowl handles MCP server configs and encrypted API keys — security matters, and we take reports seriously.

## Supported versions

Patchbowl is **pre-alpha**. There are no stable releases yet, and we patch security issues only against `main`.

| Version | Supported |
| ------- | --------- |
| `main` (rolling) | ✅ |
| Tagged releases | n/a — none yet |

Once we ship `v1.0`, we'll commit to security patches for the latest minor version and the one before it. Until then, please always run the latest `main`.

## Reporting a vulnerability

**Please do not open a public GitHub issue for security reports.**

Use one of these instead:

1. **Preferred — GitHub Private Vulnerability Reporting:**
   Go to the [Security tab](https://github.com/PatchBowl/patchbowl/security/advisories/new) and click "Report a vulnerability." This sends an encrypted report directly to the maintainers.

2. **Email:**
   `security@patchbowl.dev` *(activate once domain DNS is fully live; until then use the GitHub flow above)*

Include in your report:

- A description of the vulnerability and its impact
- Steps to reproduce (smallest reliable recipe)
- The version / commit SHA you tested against
- Any proof-of-concept code or screenshots
- Your name / handle if you'd like credit (optional)

## What you can expect from us

- **Acknowledgment within 72 hours** of receiving your report
- **Status update within 7 days** with our initial assessment
- **Resolution timeline** for confirmed issues based on severity:
  - Critical (RCE, secret exfiltration, auth bypass) → patch within 7 days
  - High (privilege escalation, encryption weakness) → patch within 30 days
  - Medium / Low → next regular release
- **Public credit** in the advisory and release notes (unless you prefer to stay anonymous)
- **No legal action** against good-faith researchers who follow this policy

## Scope

**In scope:**
- The Patchbowl server (the binary / container itself)
- Catalog YAML schema (injection, unsafe deserialization)
- Encrypted secret storage
- Client config writers (Claude Desktop, Cursor, Cline, Windsurf, Zed)
- Web UI / API endpoints

**Out of scope:**
- Vulnerabilities in MCP servers listed in the catalog — report those upstream to the server's maintainer
- Vulnerabilities in client AI tools (Claude Desktop, Cursor, etc.) — report upstream
- Issues that require a malicious operator to already have admin access to your Patchbowl instance (you've already lost)
- Denial of service via resource exhaustion against a self-hosted instance
- Social engineering of maintainers or contributors

## Disclosure policy

We follow **coordinated disclosure**:

- We will not disclose details of a vulnerability before a fix is available, unless the reporter agrees or the issue is already public.
- We ask reporters not to publicly disclose for at least **90 days** from initial report, or until the fix is shipped (whichever is sooner).
- After a fix ships, we'll publish a GitHub Security Advisory with the details, your credit, and a CVE if applicable.

## Thank you

Security research makes OSS infrastructure safer for everyone. We appreciate the time you put into making Patchbowl better.
