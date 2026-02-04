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

## Available Commands (upstream)

These commands are available in the upstream version ([kiblee/tod0](https://github.com/kiblee/tod0)):

```
todocli ls                      # List all task lists
todocli lst <list_name>         # Show tasks in a list
todocli new <task>              # Create a new task (format: "task name" or "list/task name")
todocli new <task> -r <time>    # Create task with reminder
todocli new <task> -l <list>    # Create task in a specific list (allows slashes in task name)
todocli newl <list_name>        # Create a new list
todocli complete <task>         # Mark task as completed
todocli rm <task>               # Remove a task
```

## Extended Commands (fork)

These flags are from the [underwear/tod0](https://github.com/underwear/tod0) fork. As of Feb 2026 they are not in the upstream repo — they may be merged in the future but this is not guaranteed. To verify which version is installed, run `pip show tod0` and check the source URL. If a flag is not recognized, fall back to the base commands above.

```
todocli new <task> -I                    # Mark task as important (high priority)
todocli new <task> -d <date/time>        # Set due date
todocli new <task> -R daily              # Repeat daily
todocli new <task> -R weekly             # Repeat weekly
todocli new <task> -R weekdays           # Repeat Mon-Fri
todocli new <task> -R monthly            # Repeat monthly
todocli new <task> -R yearly             # Repeat yearly
todocli new <task> -R "weekly:mon,fri"   # Repeat on specific days
todocli new <task> -R "every 2 weeks"    # Custom interval
```

When using `-R`, the `-d` flag sets when the first occurrence is due. If `-d` is omitted, it defaults to today. Example: `todocli new -R weekly -d tomorrow "Weekly review"`.

All flags can be combined: `todocli new -l "Work" -r 9:00 -d tomorrow -I -R daily "Stand-up meeting"`

## Task Format

Tasks can be specified as:
- `"task name"` — uses default list (Tasks)
- `"list name/task name"` — specifies the list
- Use `-l <list>` flag to specify list explicitly (allows task names with slashes, e.g. URLs)

## Reminder / Due Date Format

- `1h`, `30m`, `1h30m` — relative time
- `morning` — today/tomorrow at 7:00 AM
- `evening` — today/tomorrow at 6:00 PM
- `tomorrow` — tomorrow at 7:00 AM
- `9:30`, `17:15` — specific time today/tomorrow
- `9:30 am`, `10:15 pm` — 12-hour format

## Recurrence Format (fork only)

- Presets: `daily`, `weekly`, `weekdays`, `monthly`, `yearly`
- Custom intervals: `every 2 days`, `every 3 weeks`, `every 2 months`
- Weekly with days: `weekly:mon,fri`, `every 2 weeks:mon,wed,fri`
- Day abbreviations: `mon`, `tue`, `wed`, `thu`, `fri`, `sat`, `sun`

## Instructions

1. Parse the user's natural language request
2. Determine the appropriate todocli command
3. Try extended flags first if they match the request; if the command fails with an unrecognized flag error, retry with base commands only
4. Execute the command
5. Report the result clearly

If the request is ambiguous, ask for clarification. Always confirm successful operations.
