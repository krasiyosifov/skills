---
name: package-security-review
description: Review npm/yarn packages for security risks before updating. Discovers outdated packages, scans for vulnerabilities, inspects tarballs for suspicious code, and diffs old vs new versions. Use when updating dependencies, reviewing package updates, or checking if a new package version is safe.
---

# Package Security Review

## Quick Start

```bash
# 1. Discover what's outdated
npm outdated --json

# 2. Check known vulnerabilities
npm audit --json

# 3. Inspect a specific package before updating
mkdir -p /tmp/pkg-review && cd /tmp/pkg-review
rm -rf *  # clean previous run
npm pack <pkg>@<new-version> --quiet
tar -xzf <pkg>-*.tgz
grep -rn 'eval(\|exec(\|child_process\|postinstall' package/
diff -rq package/ <(npm pack <pkg>@<old-version> && tar -xzf <old-tar>)
rm -rf /tmp/pkg-review  # cleanup

# 4. Decision
# ❌ Block: critical vulns, suspicious postinstall, eval/obfuscation
# ⚠️ Review: unexpected new deps, large size increase
# ✅ Safe: all checks pass
```

See [REFERENCE.md](REFERENCE.md) for the full 5-phase playbook with commands and red flags.
