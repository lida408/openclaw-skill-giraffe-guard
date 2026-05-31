# Security Policy

## Supported Versions

The `main` branch and the latest tagged release are supported.

## Reporting a Vulnerability

Please do not open a public issue for suspected vulnerabilities.

Report security issues through GitHub private vulnerability reporting if it is enabled for this repository. If that option is not available, contact the maintainer through the email associated with the GitHub profile and include:

- A short description of the issue
- Reproduction steps or a minimal sample
- The affected file, rule, or output mode
- Whether the issue is a false negative, false positive, or scanner vulnerability

The maintainer will try to acknowledge valid reports within 7 days and publish a fix or mitigation when the issue is confirmed.

## Scope

Security reports are in scope when they affect:

- Missed detection of malicious OpenClaw skill behavior
- Incorrect SARIF or JSON output that hides security findings
- Command execution, path traversal, or unsafe temporary-file handling in the scanner
- Supply-chain risk in the repository workflow or release process

General feature requests and rule ideas can be opened as normal issues.

## 安全政策

如果你发现疑似安全漏洞，请不要直接创建公开 issue。

优先使用 GitHub 的私密漏洞报告功能。如果仓库暂未开启该功能，可以通过维护者 GitHub 资料中的邮箱联系，并提供问题描述、复现步骤、受影响文件或规则，以及它属于漏报、误报还是扫描器自身漏洞。
