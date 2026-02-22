# Security Policy

## Scope

This policy covers all repositories under the **venantvr-pubsub** organization. These projects implement publish/subscribe messaging systems, event-driven architectures, and real-time communication servers.

## Reporting a Vulnerability

If you discover a security vulnerability in any of these repositories, please report it responsibly:

1. **Do not open a public issue.** Security vulnerabilities must be reported privately.
2. Go to the affected repository's **Security** tab and click **"Report a vulnerability"** to create a private security advisory.
3. Include as much detail as possible: affected files, steps to reproduce, and potential impact.

## What We Consider Security Issues

- Authentication bypass on pub/sub servers or brokers
- Message injection or spoofing between channels
- Denial of service through malformed messages or resource exhaustion
- Unauthorized subscription to private channels
- Credential exposure in configuration files
- Insecure default configurations (open ports, no auth)

## Response

- Acknowledgment within **72 hours**
- Status update within **7 days**
- If confirmed, a fix will be prioritized and you will be credited (unless you prefer anonymity)

## Out of Scope

- Archived repositories (kept for reference, not actively maintained)
- Performance issues that do not have a security impact
- Intentional design decisions documented in the project (e.g., no auth in local-only dev servers)
