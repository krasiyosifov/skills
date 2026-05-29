# skills
Reusable AI agent skills

## Available Skills

| Skill | Description |
|-------|-------------|
| [package-security-review](package-security-review/) | Review npm/yarn packages for security risks before updating. Discovers outdated packages, scans for vulnerabilities, inspects tarballs for suspicious code, and diffs old vs new versions. |

## Adding a skill

1. Create a folder at the repository root named after the skill
2. Add a `SKILL.md` (required) and any optional supporting files (`scripts/`, `references/`, `assets/`, etc.)
3. Skills are agent-agnostic — any agent that supports the standard skill format can use them

All skills must comply with the [Agent Skills specification](https://agentskills.io/specification.md).

## Installing

### Install a specific skill

```bash
npx skills add https://github.com/krasiyosifov/skills --skill package-security-review
```

### Install all skills from this repo

```bash
npx skills add krasiyosifov/skills
```
