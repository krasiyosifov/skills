# Package Security Review Playbook

## Phase 1: Discovery & Metadata

### Check outdated status
```bash
npm outdated --json
yarn outdated
pnpm outdated --json
```

### Package basics
```bash
npm view <pkg> name version description license deprecated
npm view <pkg> maintainers
npm view <pkg> repository.url
npm view <pkg> downloads.lastMonth
npm view <pkg> time.modified
```

**Red flags:** Unknown maintainer, 0 downloads/month, recently abandoned, deprecated, no repository.

---

## Phase 2: Known Vulnerabilities

### Built-in audit
```bash
npm audit --json --audit-level=moderate
```

### Deeper scan (optional)
```bash
npx audit-ci --critical
npx snyk test
```

**Red flags:** Critical/high vulnerabilities → block the update unless a fix release is available.

---

## Phase 3: Tarball Inspection

### Download and extract
```bash
mkdir -p /tmp/pkg-review && cd /tmp/pkg-review
rm -rf *
npm pack <pkg>@<version> --quiet
tar -xzf <pkg>-*.tgz

# List contents
find package/ -type f | sort
du -sh package/
wc -l package/**/*.js package/**/*.mjs package/**/*.ts 2>/dev/null
```

**Red flags:** Huge size for claimed functionality, unexpected binary files, `.sh` scripts, hidden directories, `node_modules/` bundled in tarball.

---

## Phase 4: Suspicious Code Scan

### Post-install scripts (run at install time — most dangerous)
```bash
grep -r '"preinstall"\|"postinstall"\|"install"' package/package.json
```

### Dangerous runtime patterns
```bash
grep -rn 'eval(\|exec(\|Function(\|child_process\|require("child_process")' package/
grep -rn 'fetch.*POST\|axios.*POST\|request.*POST' package/
grep -rn 'webhook\|reverse.shell\|backdoor\|base64.*decode' package/
grep -rn 'process.env\|process.stdout\|console.log' package/
```

### Obfuscation detection
```bash
grep -rn 'eval(atob\|eval(btoa\|String.fromCharCode' package/
```

**Red flags:** Any `postinstall` script, `eval`/`exec` usage, outbound POST requests, obfuscation patterns.

---

## Phase 5: Diff & Dependency Audit

### Compare old vs new version
```bash
TMPDIR=$(mktemp -d)
cd "$TMPDIR"

# Extract old version
npm pack <pkg>@<old-version> --quiet --pack-destination=.
tar -xzf <pkg>-*.tgz
mv package old/

# Extract new version
npm pack <pkg>@<new-version> --quiet --pack-destination=.
tar -xzf <pkg>-*.tgz

# Side-by-side diff
diff -rq old/ package/
```

### New dependencies
```bash
npm ls <pkg>@<version> --all --json
```

**Red flags:** New dependencies you didn't expect, large diff in postinstall scripts, code that makes outbound network calls.

### Cleanup
```bash
rm -rf "$TMPDIR"
```

---

## Decision Matrix

| Finding | Action |
|---------|--------|
| Critical/high vuln in new version | ❌ Block — wait for patch |
| Suspicious postinstall script | ❌ Block — inspect manually |
| eval/obfuscation patterns | ❌ Block — potential supply chain attack |
| Unexpected new dependencies | ⚠️ Review each new dep |
| Large tarball size increase | ⚠️ Investigate why |
| All checks pass | ✅ Safe to update |
