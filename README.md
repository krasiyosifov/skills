# skills
Reusable AI agent skills

## Structure

```
├── package-security-review/
│   ├── SKILL.md
│   └── REFERENCE.md
├── AGENTS.md
└── README.md
```

## Adding a skill

1. Create a folder at the repository root named after the skill
2. Add a `SKILL.md` (required) and any supporting files (e.g. `REFERENCE.md`)
3. Skills are agent-agnostic — any agent that supports the standard skill format can use them

All skills must comply with the [Agent Skills specification](https://agentskills.io/client-implementation/adding-skills-support).

## Installing

### Install a specific skill

```bash
npx skills add https://github.com/krasiyosifov/skills --skill package-security-review
```

### Install all skills from this repo

```bash
npx skills add krasiyosifov/skills
```
