# Contributing

Thanks for your interest in contributing!

## Adding a New Skill

1. Fork the repository
2. Create a new directory under `skills/` with your skill name
3. Add a `SKILL.md` file following the template below
4. Update `README.md` to document your skill
5. Submit a pull request

## SKILL.md Template

```markdown
---
name: skill-name
description: Brief description of when Claude should use this skill
user-invocable: true
argument-hint: "[hint for arguments]"
---

# Skill Title

Description of what this skill does.

## Instructions

Step-by-step instructions for Claude on how to use this skill.
```

## Frontmatter Options

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Skill name (used as `/name` command) |
| `description` | Yes | When Claude should use this skill |
| `user-invocable` | No | Set to `true` to enable `/name` invocation |
| `argument-hint` | No | Hint shown in autocomplete |
| `disable-model-invocation` | No | Set to `true` to prevent Claude from auto-invoking |

## Guidelines

- Keep skills focused on a single tool or purpose
- Write clear instructions that Claude can follow
- Document prerequisites in the README
- Test your skill before submitting
- Don't include sensitive or personal information

## Questions?

Open an issue if you have questions or suggestions.
