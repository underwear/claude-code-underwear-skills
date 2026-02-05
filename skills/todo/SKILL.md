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
todo tasks                        # Default list
todo tasks Work                   # Specific list
todo tasks --due-today            # Due today
todo tasks --overdue              # Past due
todo tasks --important            # High priority
todo tasks --completed            # Done tasks
todo tasks --all                  # Everything including completed

# Create task
todo new "Task name"              # Basic task
todo new "Task" -l Work           # In specific list
todo new "Task" -d tomorrow       # With due date
todo new "Task" -r 2h             # With reminder (in 2 hours)
todo new "Task" -d mon -r 9am     # Due Monday, remind at 9am
todo new "Task" -I                # Important (high priority)
todo new "Task" -R daily          # Recurring daily
todo new "Task" -R weekly:mon,fri # Recurring on specific days
todo new "Task" -S "Step 1" -S "Step 2"  # With subtasks

# View single task
todo show "Task"                  # Show task details
todo show 0                       # Show by index

# Update task
todo update "Task" --title "New"  # Rename
todo update "Task" -d friday -I   # Change due date, make important

# Complete/Uncomplete
todo complete "Task"              # Mark complete
todo complete 0 1 2               # Complete by index (batch)
todo uncomplete "Task"            # Reopen task

# Delete
todo rm "Task"                    # Delete (asks confirmation)
todo rm "Task" -y                 # Delete (no confirmation)
```

### Subtasks (Steps)

```bash
todo new-step "Task" "Step text"      # Add step
todo list-steps "Task"                # List steps
todo complete-step "Task" "Step"      # Check off step
todo uncomplete-step "Task" "Step"    # Uncheck step
todo rm-step "Task" 0                 # Remove step by index
```

### Lists

```bash
todo lists                        # Show all lists
todo new-list "Project X"         # Create list
todo rename-list "Old" "New"      # Rename list
todo rm-list "Project X"          # Delete list (asks confirmation)
todo rm-list "Project X" -y       # Delete list (no confirmation)
```

## Task Identification

Tasks can be identified by **name**, **index**, or **ID**:

| Method | Example | Notes |
|--------|---------|-------|
| Name | `todo complete "Buy milk"` | First match wins if duplicates exist |
| Index | `todo complete 0` | Unstable — changes as tasks are added/completed |
| ID | `todo complete --id "AAMk..." -l Tasks` | Stable — requires `-l` flag |

For automation, prefer `--id` with JSON output:
```bash
ID=$(todo new "Task" -l Work --json | jq -r '.id')
todo complete --id "$ID" -l Work
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
| `every 3 weeks` | Custom interval |

When using `-R`, the `-d` flag sets the first occurrence. If omitted, defaults to today.

## JSON Output

Add `--json` to any command for machine-readable output:

```bash
todo tasks --json                 # List with full details
todo show "Task" --json           # Single task details
todo new "Task" --json            # Returns: {"action": "created", "id": "...", "title": "...", "list": "..."}
todo complete "Task" --json       # Returns: {"action": "completed", ...}
```

With `--json`: stdout contains only valid JSON. Errors go to stderr. Exit code 0 = success, 1 = error.

## Aliases

| Alias | Command |
|-------|---------|
| `t` | `tasks` |
| `n` | `new` |
| `c` | `complete` |
| `d` | `rm` |

Example: `todo c 0` = `todo complete 0`

## Instructions

1. Parse the user's natural language request
2. Determine the appropriate `todo` command
3. For delete operations, use `-y` flag to skip confirmation
4. Execute the command
5. Report the result clearly

If the request is ambiguous, ask for clarification. Always confirm successful operations.
