# AGENTS.md — Instructions for Coding Agents

This repo holds reusable, agent-agnostic AI agent skills. Each skill lives in its own folder and follows the [Agent Skills specification](https://agentskills.io/specification.md).

## Repo Structure

```
.
├── AGENTS.md            ← you are here
├── README.md            ← public-facing docs
└── <skill-name>/        ← one skill per folder
    ├── SKILL.md         ← required: frontmatter + instructions
    ├── scripts/         ← optional: executable code
    ├── references/      ← optional: detailed playbooks, supporting docs
    └── assets/          ← optional: templates, resources
```

## Skill Conventions

### SKILL.md

Must contain YAML frontmatter and markdown instructions:

```markdown
---
name: <skill-name>
description: <one-line: what it does + when to use it>
---

# <Skill Name>

<instructions>
```

**Frontmatter fields:**

| Field | Required | Constraints |
|-------|----------|-------------|
| `name` | Yes | Max 64 chars. Lowercase alphanumeric + hyphens. No leading/trailing/consecutive hyphens. Must match the parent directory name. |
| `description` | Yes | Max 1024 chars. State what the skill does and when to use it. Keywords drive discovery. |
| `license` | No | License name or reference to a bundled license file. |
| `compatibility` | No | Max 500 chars. Environment requirements (system packages, network access, etc.). |
| `metadata` | No | Arbitrary key-value pairs (e.g. `author`, `version`). |
| `allowed-tools` | No | Space-separated list of pre-approved tools (experimental). |

### Rules

1. **Agent-agnostic** — No references to specific agent implementations (pi, Claude, Cursor, etc.). Use "agent", "you", "the user".
2. **Spec-compliant** — Comply with the [Agent Skills specification](https://agentskills.io/specification.md). `name` must match the parent directory.
3. **Self-contained** — A skill is usable by reading only its own folder. References live in the same folder or in `references/`.
4. **One skill per folder** — No nesting skills inside each other's folders.
5. **Kebab-case** — Folder and `name` use kebab-case (e.g. `package-security-review`).
6. **Progressive disclosure** — Keep SKILL.md under 500 lines. Move detailed playbooks to files in `references/`.
7. **Shallow references** — File references in SKILL.md should be one level deep from the skill root. No deeply nested reference chains.

## How to Use

### Discovering Skills

Scan the repository root for available skills. The `SKILL.md` frontmatter `description` field is the primary discovery signal — pack it with relevant keywords.

### Adding a skill

1. Create a folder at the repo root named after the skill
2. Write `SKILL.md` with frontmatter — `name` must match the folder name
3. Optionally add `scripts/`, `references/`, or `assets/`
4. Keep SKILL.md under 500 lines

### Editing a skill

1. Read the skill's `SKILL.md` to understand its scope
2. Make changes within the skill's described purpose
3. Update files in `references/` if behavior changed
4. Never modify the frontmatter `name` — it's the identity key
5. Only update `description` if the skill's scope changes

## What NOT to Do

- Hardcode paths, credentials, or machine-specific configuration
- Nest skills inside each other's folders
- Modify a skill's `name` in frontmatter — it's the identity key
- Exceed 500 lines in SKILL.md — split detail into referenced files
- Use deeply nested file references — keep them one level deep
