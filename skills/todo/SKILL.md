---
name: todo
description: Manage Microsoft To Do tasks. Use when user wants to add, list, complete, remove tasks, manage subtasks, or organize lists.
user-invocable: true
argument-hint: "[action or natural language request]"
allowed-tools: Bash
---

# Microsoft To Do CLI

Manage tasks in Microsoft To Do using the `todo` command.

## User Request

$ARGUMENTS

## Commands Reference

### Tasks

```bash
# List tasks
todo tasks --json                        # Default list
todo tasks Work --json                   # Specific list
todo tasks --due-today --json            # Due today
todo tasks --overdue --json              # Past due
todo tasks --important --json            # High priority
todo tasks --completed --json            # Done tasks
todo tasks --all --json                  # Everything including completed

# Create task
todo new "Task name" --json              # Basic task
todo new "Task" -l Work --json           # In specific list
todo new "Task" -d tomorrow --json       # With due date
todo new "Task" -r 2h --json             # With reminder (in 2 hours)
todo new "Task" -d mon -r 9am --json     # Due Monday, remind at 9am
todo new "Task" -I --json                # Important (high priority)
todo new "Task" -R daily --json          # Recurring daily
todo new "Task" -R weekly:mon,fri --json # Recurring on specific days
todo new "Task" -S "Step 1" -S "Step 2" --json  # With subtasks

# View single task
todo show "Task" --json                  # Show task details
todo show 0 --json                       # Show by index

# Update task
todo update "Task" --title "New" --json  # Rename
todo update "Task" -d friday -I --json   # Change due date, make important

# Complete/Uncomplete
todo complete "Task" --json              # Mark complete
todo complete 0 1 2 --json               # Complete by index (batch)
todo uncomplete "Task" --json            # Reopen task

# Delete
todo rm "Task" -y --json                 # Delete task
```

### Subtasks (Steps)

```bash
todo new-step "Task" "Step text" --json      # Add step
todo list-steps "Task" --json                # List steps
todo complete-step "Task" "Step" --json      # Check off step
todo uncomplete-step "Task" "Step" --json    # Uncheck step
todo rm-step "Task" 0 --json                 # Remove step by index
```

### Lists

```bash
todo lists --json                        # Show all lists
todo new-list "Project X" --json         # Create list
todo rename-list "Old" "New" --json      # Rename list
todo rm-list "Project X" -y --json       # Delete list
```

## JSON Response Structures

**`todo tasks --json`:**
```json
{
  "list": "Tasks",
  "tasks": [
    {
      "id": "AAMkADU3...",
      "title": "Buy groceries",
      "status": "notStarted",
      "importance": "normal",
      "due_date": null,
      "reminder": null,
      "recurrence": null,
      "steps": []
    }
  ]
}
```

**`todo lists --json`:**
```json
{
  "lists": [
    {"id": "AAMk...", "name": "Tasks", "is_owner": true},
    {"id": "AAMk...", "name": "Work", "is_owner": true}
  ]
}
```

**Write commands (`new`, `complete`, `rm`):**
```json
{"action": "created", "id": "AAMk...", "title": "Task", "list": "Tasks"}
{"action": "completed", "id": "AAMk...", "title": "Task", "list": "Tasks"}
{"action": "removed", "id": "AAMk...", "title": "Task", "list": "Tasks"}
```

## Task Identification

| Method | Stability | When to Use |
|--------|-----------|-------------|
| `--id "AAMk..."` | Stable | Multi-step operations, automation |
| Index (`0`, `1`) | Unstable | Quick interactive commands only |
| Name (`"Task"`) | Unstable | Unique task names only |

```bash
# Use --id with -l (list context required)
todo complete --id "AAMkADU3..." -l Tasks --json
todo update --id "AAMkADU3..." --title "New title" -l Work --json
todo rm --id "AAMkADU3..." -l Tasks -y --json
```

## Date & Time Formats

| Type | Examples |
|------|----------|
| Relative | `1h`, `30m`, `2d`, `1h30m` |
| Time | `9:30`, `9am`, `17:00`, `5:30pm` |
| Days | `tomorrow`, `monday`, `fri` |
| Date | `2026-12-31`, `31.12.2026`, `12/31/2026` |
| Keywords | `morning` (7:00), `evening` (18:00) |

## Recurrence Patterns

| Pattern | Description |
|---------|-------------|
| `daily` | Every day |
| `weekly` | Every week |
| `monthly` | Every month |
| `yearly` | Every year |
| `weekdays` | Monday to Friday |
| `weekly:mon,wed,fri` | Specific days |
| `every 2 days` | Custom interval |

## Aliases

| Alias | Command |
|-------|---------|
| `t` | `tasks` |
| `n` | `new` |
| `c` | `complete` |
| `d` | `rm` |

## Instructions

1. Parse the user's natural language request
2. Determine the appropriate `todo` command
3. **Always use `--json` flag** — all examples above include it
4. **Always use `-y` flag** with `rm` commands
5. For multi-step operations, capture ID from JSON response:
   ```bash
   ID=$(todo new "Task" -l Work --json | jq -r '.id')
   todo complete --id "$ID" -l Work --json
   ```
6. Parse JSON response and report result clearly to the user

If the request is ambiguous, ask for clarification.
