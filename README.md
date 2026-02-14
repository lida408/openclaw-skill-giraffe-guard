# 🛡️ OpenClaw Skill Security Auditor

A security scanner for [OpenClaw](https://github.com/openclaw/openclaw) skills — detect supply chain attacks, malicious code, and suspicious patterns before they compromise your system.

> Born from a real supply chain poisoning incident in the OpenClaw community. Stay safe.

## Features

- **17 detection rules** covering the full supply chain attack surface
- **Context-aware** — distinguishes documentation from executable code (low false positives)
- **Zero dependencies** — only uses bash, grep, sed, find, file, awk
- **Cross-platform** — macOS (BSD) and Linux (GNU) compatible
- **Multiple output formats** — colored terminal, JSON reports
- **Whitelist support** — suppress known-safe findings
- **Verbose mode** — show surrounding context lines for each finding

## Quick Start

### As an OpenClaw Skill

```bash
# Clone into your OpenClaw skills directory
git clone https://github.com/huixingxiaohuoban/openclaw-skill-security-audit.git \
  ~/.openclaw/workspace/skills/security-audit

# Scan your skills
bash ~/.openclaw/workspace/skills/security-audit/scripts/audit.sh ~/.openclaw/workspace/skills/
```

### Standalone

```bash
git clone https://github.com/huixingxiaohuoban/openclaw-skill-security-audit.git
cd openclaw-skill-security-audit
bash scripts/audit.sh /path/to/scan
```

## Usage

```bash
# Basic scan
bash scripts/audit.sh /path/to/skills

# Verbose mode (show context lines around findings)
bash scripts/audit.sh --verbose /path/to/skills

# JSON output (for CI/CD integration)
bash scripts/audit.sh --json /path/to/skills

# With whitelist
bash scripts/audit.sh --whitelist whitelist.txt /path/to/skills

# Custom context lines (default: 2)
bash scripts/audit.sh --verbose --context 5 /path/to/skills
```

## Detection Rules

### 🔴 Critical (immediate action required)

| # | Rule | Description |
|---|------|-------------|
| 1 | pipe-execution | Remote code piped to shell (`curl \| bash`) |
| 2 | base64-decode-pipe | Base64 decoded and executed |
| 3 | security-bypass | macOS Gatekeeper/SIP bypass |
| 5 | tor-onion-address | Tor hidden service addresses |
| 5 | reverse-shell | Reverse shell patterns |
| 7 | file-type-disguise | Binary masquerading as text file |
| 8 | ssh-key-exfiltration | SSH key theft via network |
| 8 | cloud-credential-access | Cloud credential access (AWS/GCP/Azure) |
| 8 | env-exfiltration | Environment variables sent over network |
| 9 | anti-sandbox | Anti-debug/anti-sandbox techniques |
| 10 | covert-downloader | One-liner downloaders (Python/Node/Ruby/Perl/PowerShell) |
| 11 | persistence-launchagent | macOS LaunchAgent persistence |
| 13 | string-concat-bypass | String concatenation to evade detection |
| 15 | env-file-leak | `.env` file containing real secrets |
| 16 | typosquat-npm/pip | Typosquatting package names |
| 17 | malicious-postinstall | Malicious lifecycle scripts |

### 🟡 Warning (manual review recommended)

| # | Rule | Description |
|---|------|-------------|
| 2 | long-base64-string | Suspiciously long Base64 strings |
| 4 | dangerous-permissions | Dangerous permission changes |
| 5 | suspicious-network-ip | Direct IP connections (non-local) |
| 5 | netcat-listener | Netcat listeners |
| 6 | covert-exec-eval | Suspicious eval() calls |
| 11 | cron-injection | Cron/launchctl/systemd injection |
| 12 | hidden-executable | Hidden executable files |
| 13 | hex/unicode-obfuscation | Hex/Unicode escape obfuscation |
| 14 | symlink-sensitive | Symlinks pointing to sensitive locations |
| 16 | custom-registry | Non-official package registries |

## Whitelist File Format

Create a whitelist file to suppress known-safe findings:

```txt
# Whitelist entire file
path/to/trusted-file.sh

# Whitelist specific line number
path/to/file.sh:42

# Whitelist specific rule for a file
path/to/file.sh:pipe-execution
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | ✅ Clean — no findings |
| 1 | 🟡 Warnings found |
| 2 | 🔴 Critical findings |

## CI/CD Integration

```yaml
# GitHub Actions example
- name: Security Audit
  run: |
    bash scripts/audit.sh --json ./skills > audit-report.json
    EXIT_CODE=$?
    if [ $EXIT_CODE -eq 2 ]; then
      echo "::error::Critical security findings detected!"
      exit 1
    fi
```

## Automation with OpenClaw

Add to your `TOOLS.md` to enforce scanning on every skill install:

```markdown
## 🛡️ Skill Security Audit (mandatory)
Every new skill must be scanned before activation:
1. Run: `bash skills/security-audit/scripts/audit.sh <new-skill-path>`
2. Exit 0 → safe to use
3. Exit 1 → report warnings to user
4. Exit 2 → block activation, notify user
```

Schedule daily scans via OpenClaw cron:
```
0 4 * * * bash skills/security-audit/scripts/audit.sh /path/to/skills
```

## License

[Apache License 2.0](LICENSE)

## Contributing

Issues and PRs welcome. When adding new detection rules:

1. Add the check function in `scripts/audit.sh`
2. Call it from `scan_file()` (file-level) or `main()` (directory-level)
3. Update `SKILL.md` rule table
4. Test against both clean skills and malicious samples
5. Ensure zero false positives on standard OpenClaw bundled skills
