# skills
Reusable AI agent skills

## Structure

```
├── community/          ← Skills from the ecosystem / third-party
│   └── package-security-review/
│       ├── SKILL.md
│       └── REFERENCE.md
├── custom/             ← Your own custom skills (create as needed)
└── templates/          ← Skill scaffolding templates (create as needed)
```

## Adding a skill

1. Create a folder under `community/` or `custom/` named after the skill
2. Add a `SKILL.md` (required) and any supporting files (e.g. `REFERENCE.md`)
3. Skills are agent-agnostic — any agent that supports the standard skill format can use them

All skills must comply with the [Agent Skills specification](https://agentskills.io/client-implementation/adding-skills-support).
