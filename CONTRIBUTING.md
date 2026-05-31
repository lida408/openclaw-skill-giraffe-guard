# Contributing

Thanks for helping improve Giraffe Guard.

This project is a security scanner, so small, reviewed changes are preferred over broad rewrites.

## Good Contributions

- New detection rules with clean and malicious test samples
- False-positive reductions that keep malicious samples detected
- SARIF, JSON, CI, or documentation fixes
- Cross-platform fixes for macOS and Linux

## Adding a Detection Rule

1. Add the check in `scripts/audit.sh` or `scripts/ast_analyzer.py`.
2. Register it in the scan flow.
3. Confirm it appears in `bash scripts/audit.sh --list-rules`.
4. Test against at least one malicious sample and one clean sample.
5. Run a self-scan before opening a PR:

```bash
bash scripts/audit.sh --quiet --fail-on CRITICAL .
```

## Pull Request Checklist

- Keep the change focused on one problem.
- Do not add third-party dependencies unless there is a strong security reason.
- Update `README.md` or `SKILL.md` when behavior changes.
- Include the command output used to verify the change.

## 贡献说明

欢迎提交改进。这个项目是安全扫描器，因此更偏好小而清楚的改动。

新增检测规则时，请确认规则已经出现在 `--list-rules` 输出中，并同时用恶意样本和正常样本验证。提交前请运行：

```bash
bash scripts/audit.sh --quiet --fail-on CRITICAL .
```
