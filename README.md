# Claude Code Underwear Skills

A collection of useful skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Available Skills

| Skill | Description |
|-------|-------------|
| `/todo` | Manage Microsoft To Do tasks via natural language |

## Installation

### Quick Install (All Skills)

```bash
# Clone the repository
git clone https://github.com/underwear/claude-code-underwear-skills.git

# Copy all skills to your Claude Code skills directory
cp -r claude-code-underwear-skills/skills/* ~/.claude/skills/

# Clean up
rm -rf claude-code-underwear-skills
```

### Install Individual Skill

```bash
# Create skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# Download specific skill (example: todo)
curl -fsSL https://raw.githubusercontent.com/underwear/claude-code-underwear-skills/main/skills/todo/SKILL.md -o ~/.claude/skills/todo/SKILL.md --create-dirs
```

### Verify Installation

Restart Claude Code and run:

```
/skills
```

You should see the installed skills listed.

## Skills Documentation

### `/todo` — Microsoft To Do Integration

Manage your Microsoft To Do tasks using natural language.

#### Prerequisites

1. Install [tod0](https://github.com/kiblee/tod0):
   ```bash
   pip install tod0
   ```

2. Register for an API key following [these instructions](https://github.com/kiblee/tod0/blob/master/GET_KEY.md)

3. Run `tod0` once to complete authentication

#### Usage Examples

```
/todo show all my lists
/todo show tasks in Shopping list
/todo add "Buy groceries" to Shopping list
/todo add "Call mom" with reminder in 2 hours
/todo complete "Buy groceries" in Shopping
/todo remove "Old task"
/todo create a new list called Work
```

#### Supported Operations

- **List tasks** — view all lists or tasks in a specific list
- **Add tasks** — create new tasks with optional reminders
- **Complete tasks** — mark tasks as done
- **Remove tasks** — delete tasks
- **Create lists** — make new task lists

## Uninstall

Remove individual skill:

```bash
rm -rf ~/.claude/skills/todo
```

Remove all skills from this collection:

```bash
rm -rf ~/.claude/skills/todo
# Add more as skills are added to this repo
```

## Adding New Skills

Want to contribute? See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Skill Structure

```
skills/
└── your-skill-name/
    └── SKILL.md
```

### SKILL.md Template

```markdown
---
name: your-skill-name
description: When Claude should use this skill
user-invocable: true
argument-hint: "[optional arguments hint]"
---

# Skill Title

Instructions for Claude...
```

## License

MIT License — see [LICENSE](LICENSE) for details.
