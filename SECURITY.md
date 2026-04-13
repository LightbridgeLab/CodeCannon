# Security Policy

## Supported Versions

| Version | Supported          |
|---------|--------------------|
| Latest  | :white_check_mark: |
| Older   | :x:                |

Only the latest release receives security fixes. We recommend always using the most recent version.

## Reporting a Vulnerability

Code Cannon is a developer tool that generates static configuration files. It does not run a server, handle user credentials, or process sensitive data. The attack surface is limited to the sync script and generated output.

If you discover a security issue, please **open a GitHub Issue** in this repository with the label `security`. Include:

- A description of the vulnerability
- Steps to reproduce
- The potential impact

We will acknowledge the report within 7 days and aim to provide a fix or mitigation within 30 days, depending on severity.

For issues that could enable supply-chain attacks (e.g., generating malicious output files), please email the maintainers directly rather than opening a public issue. Contact information is available in the repository's GitHub profile.
