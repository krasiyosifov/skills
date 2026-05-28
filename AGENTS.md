# AGENTS.md — Instructions for Coding Agents

## About This Repo

This repository contains reusable, agent-agnostic AI agent skills. Skills follow the [Agent Skills specification](https://agentskills.io/specification) and are designed to work across any compatible agent implementation.

## Repository Structure

```
skills/
├── AGENTS.md            ← You are here
├── README.md            ← Public-facing documentation
├── community/           ← Community / third-party skills
│   └── <skill-name>/
│       ├── SKILL.md           ← Required: skill definition
│       ├── REFERENCE.md       ← Optional: extended reference
│       └── references/        ← Optional: additional reference files
├── custom/              ← Your own custom skills
└── templates/           ← Skill scaffolding templates
```

## Skill Conventions

### File Structure

Every skill lives in its own folder and must contain at minimum:

- **`SKILL.md`** — The skill definition with frontmatter (`name`, `description`) and instructions
- **`REFERENCE.md`** — Optional extended reference, commands, or playbooks

### SKILL.md Format

```markdown
---
name: <skill-name>
description: <one-line description of when and what this skill does>
---

# <Skill Name>

<Instructions for the agent>
```

### Frontmatter

| Field | Required | Constraints |
|-------|----------|-------------|
| `name` | Yes | Max 64 chars. Lowercase alphanumeric + hyphens only. No leading/trailing/consecutive hyphens. Must match the parent directory name. |
| `description` | Yes | Max 1024 chars. Must state what the skill does and when to use it. |
| `license` | No | License name or reference to a bundled license file. |
| `compatibility` | No | Max 500 chars. Environment requirements (system packages, network access, etc.). |
| `metadata` | No | Arbitrary key-value pairs (e.g. `author`, `version`). |
| `allowed-tools` | No | Space-separated list of pre-approved tools (experimental). |

### Rules

1. **Agent-agnostic** — Skills must not reference any specific agent implementation (pi, Claude, Cursor, etc.). Use generic terms like "agent", "you", "the user".
2. **Spec-compliant** — All skills must comply with the [Agent Skills specification](https://agentskills.io/specification). The `name` must match the parent directory, and follow all naming constraints.
3. **Self-contained** — A skill should be usable by reading only its own folder. Reference files live in the same folder or in a `references/` subdirectory.
4. **Descriptive frontmatter** — The `description` must clearly state what the skill does and when to use it, with keywords for discovery.
5. **One skill per folder** — Each folder represents exactly one skill.
6. **Kebab-case names** — Folder and `name` must use kebab-case (e.g. `package-security-review`).
7. **Progressive disclosure** — Keep SKILL.md under 500 lines. Move detailed reference material to `REFERENCE.md` or files in `references/`. Agents load them on demand.
8. **Shallow references** — File references in SKILL.md should be one level deep from the skill root. Avoid deeply nested reference chains.

## How to Use

### Discovering Skills

Scan the `community/` and `custom/` directories for available skills. The `SKILL.md` frontmatter `description` field is the primary discovery signal.

### Adding a New Skill

1. Create a folder under `community/` (for shared skills) or `custom/` (for personal skills)
2. Write `SKILL.md` with proper frontmatter — `name` must match the folder name and follow naming constraints
3. Add `REFERENCE.md` or a `references/` directory for extended reference material
4. Keep SKILL.md concise (under 500 lines); move detailed playbooks to referenced files
5. Optionally add supporting files (scripts, templates, etc.)

### Editing an Existing Skill

1. Read the skill's `SKILL.md` to understand its current scope
2. Make changes that stay within the skill's described purpose
3. Update `REFERENCE.md` if behavior or commands have changed
4. Keep the frontmatter `name` unchanged; update `description` only if the scope changes

## What NOT to Do

- Do not add agent-specific language (no "pi", "Claude", "Cursor", etc.)
- Do not nest skills inside each other's folders
- Do not put non-skill files inside a skill folder
- Do not modify a skill's `name` in frontmatter — it's the identity key and must match the folder
- Do not hardcode paths, credentials, or machine-specific configuration
- Do not put deeply nested file references in SKILL.md — keep them one level deep
- Do not exceed 500 lines in SKILL.md — split detail into referenced files
