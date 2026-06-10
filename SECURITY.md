# Security Policy

The security of DLC OS — a platform that handles orders, customer data, and
payments — is a first-class concern. Thank you for helping keep it and its users
safe.

## Supported Versions

DLC OS is pre-alpha. Until a `1.0` release, only the `main` branch is supported
for security fixes. After `1.0`, the latest minor release line will receive
security patches.

| Version | Supported |
|---------|-----------|
| `main`  | ✅ |
| < 1.0   | ⚠️ best-effort |

## Reporting a Vulnerability

**Please do not open a public issue for security vulnerabilities.**

Instead, report privately via one of:

1. **GitHub Security Advisories** — "Report a vulnerability" on the Security tab (preferred).
2. **Email** — `security@dlc-os.dev` (PGP key published on the website).

Please include:

- A description of the vulnerability and its impact
- Steps to reproduce (proof-of-concept if possible)
- Affected component(s) and version/commit
- Any suggested remediation

## What to expect

| Stage | Target |
|---|---|
| Acknowledgement of your report | within **48 hours** |
| Initial assessment & severity | within **5 business days** |
| Fix or mitigation plan | depends on severity (critical issues prioritized) |
| Public disclosure | coordinated with you, after a fix is available |

We follow **coordinated disclosure**. We will credit you in the advisory (unless
you prefer to remain anonymous) and never take legal action against good-faith
research that respects user privacy and avoids data destruction.

## Scope

In scope: the DLC OS core, official channel integrations, the dashboard, and
official deployment manifests.

Out of scope: third-party services we integrate with (report those to the
respective vendor), social-engineering, and denial-of-service testing against
shared infrastructure.

## Security by design

DLC OS is built with security as a foundation, not an afterthought. See the
full **[Security Architecture](./docs/09-security-architecture.md)** for our
approach to authentication, RBAC, encryption, audit logging, rate limiting, and
fraud detection.
