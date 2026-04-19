# Security Policy

## Scope

Agentify is a collection of markdown files and workflow definitions. It does not:
- Execute code autonomously
- Handle credentials or secrets
- Make network requests
- Store data outside the repository it is installed in

However, the AI platform Agentify runs on (Claude, Codex, etc.) may send file contents to external APIs. Review your AI platform's privacy policy before installing Agentify in repositories containing sensitive data.

## Reporting a vulnerability

If you discover a security issue — for example, a workflow definition that could be manipulated to cause an AI agent to exfiltrate data or bypass boundary rules — please report it privately.

**Do not open a public GitHub issue for security vulnerabilities.**

Report via:
- GitHub: use the [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability) feature on this repository
- Email: open an issue asking for a contact address if you cannot use GitHub's private reporting

We will acknowledge receipt within 7 days and work with you on a fix and disclosure timeline.

## Supported versions

Only the latest commit on `main` is supported.
