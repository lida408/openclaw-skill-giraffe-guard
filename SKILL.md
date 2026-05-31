# 🦒 Giraffe Guard — 长颈鹿卫士

Scan OpenClaw skill directories and Git repositories for supply-chain attacks, malicious code, secret leaks, and suspicious patterns before installation.

扫描 OpenClaw skill 目录和 Git 仓库，在安装前发现供应链投毒、恶意代码、密钥泄露和可疑模式。

## Features / 功能

- 55+ detection rules: grep-based checks plus Python AST semantic analysis / 55+ 条检测规则：grep 检测 + Python AST 语义分析
- Pre-install scanning for Git repositories or local skill directories / 支持安装前扫描 Git 仓库或本地 skill 目录
- Context-aware matching to reduce documentation false positives / 上下文感知，降低文档描述造成的误报
- Colored terminal, JSON, and SARIF outputs / 支持彩色终端、JSON 和 SARIF 输出
- Severity thresholds, rule skipping, whitelist, quiet mode, and verbose context / 支持严重级别过滤、规则跳过、白名单、静默模式和详细上下文
- Compatible with macOS and Linux, with no third-party runtime dependencies / 兼容 macOS 和 Linux，无第三方运行时依赖

## Usage / 使用方法

### Scan a skill directory / 扫描目录

```bash
{baseDir}/scripts/audit.sh /path/to/skills
```

### Pre-install scan / 安装前扫描

```bash
{baseDir}/scripts/audit.sh --pre-install https://github.com/user/skill-repo.git
```

### Quiet CI mode / CI 静默模式

```bash
{baseDir}/scripts/audit.sh --quiet --fail-on CRITICAL /path/to/skills
```

### JSON report / JSON 报告

```bash
{baseDir}/scripts/audit.sh --json /path/to/skills
```

### SARIF report / SARIF 报告

```bash
{baseDir}/scripts/audit.sh --sarif /path/to/skills > results.sarif
```

### Verbose mode / 详细模式

```bash
{baseDir}/scripts/audit.sh --verbose --context 3 /path/to/skills
```

### With whitelist / 使用白名单

```bash
{baseDir}/scripts/audit.sh --whitelist whitelist.txt /path/to/skills
```

### Skip directories / 跳过目录

```bash
{baseDir}/scripts/audit.sh --skip-dir node_modules --skip-dir vendor /path/to/skills
```

## Detection Rules / 检测规则

Run the command below for the maintained rule list:

```bash
{baseDir}/scripts/audit.sh --list-rules
```

规则列表以脚本输出为准，避免文档和检测逻辑不一致。

## Exit Codes / 退出码

- `0` — Clean / 安全
- `1` — Warnings / 有警告
- `2` — Critical findings / 有严重发现

## Dependencies / 依赖

Runtime scanning uses system tools such as bash, grep, sed, find, awk, and Python stdlib for AST analysis.

运行时使用系统工具，如 bash、grep、sed、find、awk；AST 分析只使用 Python 标准库。
