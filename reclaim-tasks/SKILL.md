---
name: reclaim-tasks
description: Manage tasks in Reclaim.ai calendar scheduling app. Use when creating, updating, listing, completing, or deleting Reclaim tasks, or working with calendar scheduling, task priorities, time blocking, or task duration management.
---

# Reclaim Tasks

Manage CRUD operations for tasks in Reclaim.ai using the `reclaim` CLI.

## Installation Check

**IMPORTANT**: Before using any Reclaim commands, always check if the `reclaim` CLI is installed:

```bash
which reclaim
```

If not installed, install it automatically based on the system:

**All systems** (requires Ruby):
```bash
gem install reclaim
```

If automatic installation fails, inform the user that they need to install Ruby first, then run `gem install reclaim`.

After installation, verify it works:
```bash
reclaim --help
```

## Mandatory Confirmation Workflow

**CRITICAL**: For ALL write operations (create, update, complete, delete), you MUST:

1. Parse the user's request and construct the `reclaim` command
2. Use the AskUserQuestion tool to show the command and get confirmation
3. Only execute the command after user approval

**Read operations** (list, get, list-schemes) can execute immediately without confirmation.

## Quick Command Reference

### Read Operations (no confirmation needed)
```bash
reclaim                    # List active tasks (default)
reclaim list active        # List active tasks (explicit)
reclaim list completed     # List completed tasks
reclaim list overdue       # List overdue tasks
reclaim get TASK_ID        # Get task details
reclaim list-schemes       # List available time schemes
```

### Write Operations (REQUIRE confirmation)
```bash
# Create
reclaim create --title "TITLE" [OPTIONS]

# Update
reclaim update TASK_ID [OPTIONS]

# Complete
reclaim complete TASK_ID

# Delete
reclaim delete TASK_ID
```

## Common Options

- `--title TITLE` - Task title
- `--due DATE` - Due date (YYYY-MM-DD or YYYY-MM-DDTHH:MM:SS, or "none" to clear)
- `--priority P1|P2|P3|P4` - Task priority
- `--duration HOURS` - Duration in hours (0.25 = 15min, 1.5 = 90min)
- `--split [CHUNK_SIZE]` - Allow task splitting (optional min chunk size)
- `--defer DATE` - Start after this date (or "none" to clear)
- `--start DATE` - Specific start time (or "none" to clear)
- `--time-scheme SCHEME` - Time scheme ID or alias (work, personal, etc.)
- `--notes TEXT` - Task notes/description

## Example Workflow with Confirmation

**User request**: "Create a task called 'Write proposal' due Friday, P1 priority, 3 hours"

**Your response**:
1. Construct command: `reclaim create --title "Write proposal" --due 2025-11-07 --priority P1 --duration 3`
2. Use AskUserQuestion to confirm:
   ```
   Ready to create this Reclaim task:

   Command: reclaim create --title "Write proposal" --due 2025-11-07 --priority P1 --duration 3

   This will create a P1 task with 3 hours duration due on 2025-11-07.

   Proceed?
   ```
3. After approval, execute the command

## Additional Resources

- [EXAMPLES.md](EXAMPLES.md) - Comprehensive examples for all workflows
- [REFERENCE.md](REFERENCE.md) - Complete option and command reference

## Date Formats

- Standard: `YYYY-MM-DD` (e.g., 2025-11-07)
- With time: `YYYY-MM-DDTHH:MM:SS` (e.g., 2025-11-07T14:30:00)
- Clear date: `none`, `clear`, or `null`

## Priority Levels

- `P1` - Highest priority
- `P2` - High priority
- `P3` - Medium priority
- `P4` - Low priority

## Time Scheme Aliases

- `work`, `working hours`, `business hours` → Work time schemes
- `personal`, `off hours`, `private` → Personal time schemes
