---
name: todo
description: Manage Microsoft To Do tasks via todocli. Use when user wants to add, list, complete, or remove tasks.
user-invocable: true
argument-hint: "[action or natural language request]"
---

# Microsoft To Do Integration

Manage tasks in Microsoft To Do using the `todocli` command-line tool.

## User Request

$ARGUMENTS

## Available Commands

```
todocli ls                      # List all task lists
todocli lst <list_name>         # Show tasks in a list
todocli new <task>              # Create a new task (format: "task name" or "list/task name")
todocli new <task> -r <time>    # Create task with reminder
todocli newl <list_name>        # Create a new list
todocli complete <task>         # Mark task as completed
todocli rm <task>               # Remove a task
```

## Task Format

Tasks can be specified as:
- `"task name"` — uses default list (Tasks)
- `"list name/task name"` — specifies the list
- Task number (use `-n` flag to see numbers)

## Reminder Time Format

- `1h`, `30m`, `1h30m` — relative time
- `morning` — today/tomorrow at 7:00 AM
- `evening` — today/tomorrow at 6:00 PM
- `tomorrow` — tomorrow at 7:00 AM
- `9:30`, `17:15` — specific time today/tomorrow
- `9:30 am`, `10:15 pm` — 12-hour format

## Instructions

1. Parse the user's natural language request
2. Determine the appropriate todocli command
3. Execute the command
4. Report the result clearly

If the request is ambiguous, ask for clarification. Always confirm successful operations.
