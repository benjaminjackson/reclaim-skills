# Reclaim Tasks: Complete Reference

Complete reference documentation for the `reclaim` CLI.

## Commands

### list [FILTER]
List tasks with optional filter.

**Filters:**
- `active` (default when no command given) - Lists scheduled and in-progress tasks
- `completed` - Lists completed tasks
- `overdue` - Lists tasks past their due date

**Examples:**
```bash
reclaim                # List active tasks (default)
reclaim list           # List active tasks (explicit)
reclaim list active    # List active tasks
reclaim list completed # List completed tasks
reclaim list overdue   # List overdue tasks
```

### create
Create a new task. Requires `--title` at minimum.

**Required:**
- `--title TITLE` - Task title

**Optional:** See "Task Options" section below

**Examples:**
```bash
reclaim create --title "My task"
reclaim create --title "Important work" --due 2025-11-15 --priority P1
```

### get TASK_ID
Get detailed information about a specific task.

**Arguments:**
- `TASK_ID` - The unique identifier for the task

**Examples:**
```bash
reclaim get abc123
```

### update TASK_ID
Update an existing task. At least one option must be provided.

**Arguments:**
- `TASK_ID` - The unique identifier for the task

**Options:** See "Task Options" section below

**Examples:**
```bash
reclaim update abc123 --title "Updated title"
reclaim update abc123 --priority P1 --due 2025-11-20
```

### complete TASK_ID
Mark a task as complete (sets status to ARCHIVED).

**Arguments:**
- `TASK_ID` - The unique identifier for the task

**Examples:**
```bash
reclaim complete abc123
```

**Note:** This sets the task to ARCHIVED status, which represents a truly completed task in Reclaim.

### delete TASK_ID
Permanently delete a task. This action cannot be undone.

**Arguments:**
- `TASK_ID` - The unique identifier for the task

**Examples:**
```bash
reclaim delete abc123
```

**Warning:** This is permanent deletion. Use `complete` if you want to mark a task as done while preserving it.

### list-schemes
List all available time schemes for the account.

**Examples:**
```bash
reclaim list-schemes
```

### help
Show help message with command and option reference.

**Examples:**
```bash
reclaim help
reclaim --help
```

## Task Options

Options available for `create` and `update` commands.

### --title TITLE
Task title text.

**Type:** String
**Required for:** create
**Optional for:** update

**Examples:**
```bash
--title "Write quarterly report"
--title "Review PR #123"
```

### --due DATE
Task due date. Can be a date or date-time.

**Type:** Date (YYYY-MM-DD) or DateTime (YYYY-MM-DDTHH:MM:SS)
**Special values:** `none`, `clear`, `null` (to remove due date)

**Examples:**
```bash
--due 2025-11-30
--due 2025-11-30T17:00:00
--due none  # Clear the due date
```

### --priority PRIORITY
Task priority level.

**Type:** P1, P2, P3, or P4
**Default:** P3 (when not specified)

**Priority levels:**
- `P1` - Highest priority (most urgent/important)
- `P2` - High priority
- `P3` - Medium priority (default)
- `P4` - Low priority

**Examples:**
```bash
--priority P1
--priority P4
```

### --duration HOURS
Task duration in hours.

**Type:** Decimal number
**Common values:**
- `0.25` - 15 minutes
- `0.5` - 30 minutes
- `0.75` - 45 minutes
- `1` - 1 hour
- `1.5` - 90 minutes
- `2` - 2 hours
- `4` - 4 hours (half day)
- `8` - 8 hours (full day)

**Examples:**
```bash
--duration 2
--duration 0.5
--duration 1.5
```

### --split [CHUNK_SIZE]
Allow Reclaim to split the task into smaller chunks across multiple time slots.

**Type:** Optional decimal number (minimum chunk size in hours)
**Default:** When flag is present without value, Reclaim uses its default minimum

**Examples:**
```bash
--split              # Allow splitting with default minimum
--split 0.5          # Allow splitting, minimum 30-minute chunks
--split 1            # Allow splitting, minimum 1-hour chunks
```

**Note:** Use with `--min-chunk` and `--max-chunk` for finer control.

### --min-chunk HOURS
Minimum chunk size when task splitting is enabled.

**Type:** Decimal number (hours)
**Requires:** `--split` flag

**Examples:**
```bash
--split --min-chunk 0.5   # Minimum 30-minute chunks
--split --min-chunk 1     # Minimum 1-hour chunks
```

### --max-chunk HOURS
Maximum chunk size when task splitting is enabled.

**Type:** Decimal number (hours)
**Requires:** `--split` flag

**Examples:**
```bash
--split --max-chunk 2     # Maximum 2-hour chunks
--split --min-chunk 0.5 --max-chunk 2  # Between 30min and 2 hours
```

### --min-work HOURS
Minimum total work duration.

**Type:** Decimal number (hours)

**Examples:**
```bash
--min-work 1
--min-work 0.5
```

### --max-work HOURS
Maximum total work duration.

**Type:** Decimal number (hours)

**Examples:**
```bash
--max-work 4
--max-work 2
```

### --defer DATE
Start task after this date/time. Task won't be scheduled before this date.

**Type:** Date (YYYY-MM-DD) or DateTime (YYYY-MM-DDTHH:MM:SS)
**Special values:** `none`, `clear`, `null` (to remove defer date)
**Alias:** `--snooze` (same functionality)

**Examples:**
```bash
--defer 2025-11-15
--defer 2025-11-15T09:00:00
--defer none  # Clear the defer date
```

### --snooze DATE
Synonym for `--defer`. Start task after this date/time.

**Type:** Date (YYYY-MM-DD) or DateTime (YYYY-MM-DDTHH:MM:SS)
**Special values:** `none`, `clear`, `null` (to remove snooze date)

**Examples:**
```bash
--snooze 2025-11-20
--snooze none  # Clear the snooze date
```

### --start DATE
Specific start time for the task. Locks the task to a specific calendar slot.

**Type:** DateTime (YYYY-MM-DDTHH:MM:SS)
**Special values:** `none`, `clear`, `null` (to remove specific start time)

**Examples:**
```bash
--start 2025-11-15T14:00:00
--start none  # Clear the specific start time
```

**Note:** This pins the task to a specific calendar time rather than letting Reclaim schedule it flexibly.

### --time-scheme SCHEME
Time scheme that defines when the task can be scheduled.

**Type:** Time scheme ID or alias

**Common aliases:**
- `work`, `working hours`, `business hours` - Finds schemes containing 'work'
- `personal`, `off hours`, `private` - Finds schemes containing 'personal'

**Examples:**
```bash
--time-scheme work
--time-scheme personal
--time-scheme ts_abc123def  # Specific scheme ID
```

**Note:** Use `reclaim list-schemes` to see available time schemes.

### --private BOOL
Make the task private (hidden from others who can see your calendar).

**Type:** Boolean (true or false)

**Examples:**
```bash
--private true
--private false
```

### --category CATEGORY
Event category for grouping and filtering.

**Type:** String

**Examples:**
```bash
--category "Meetings"
--category "Deep Work"
--category "Planning"
```

### --color COLOR
Color for the event on your calendar.

**Type:** Color name or code

**Examples:**
```bash
--color blue
--color red
--color green
```

### --notes TEXT
Additional notes or description for the task.

**Type:** String (may require quotes if contains spaces)

**Examples:**
```bash
--notes "Need to include Q3 metrics"
--notes "Follow up with John about API changes"
```

## Date and Time Formats

### Date Format
**Format:** `YYYY-MM-DD`
**Examples:**
- `2025-11-15`
- `2025-12-31`
- `2026-01-01`

### DateTime Format
**Format:** `YYYY-MM-DDTHH:MM:SS`
**Examples:**
- `2025-11-15T14:30:00` (2:30 PM)
- `2025-11-15T09:00:00` (9:00 AM)
- `2025-11-20T16:45:00` (4:45 PM)

### Clearing Dates
To remove a date field, use any of these special values:
- `none`
- `clear`
- `null`

**Examples:**
```bash
reclaim update abc123 --due none
reclaim update abc123 --defer clear
reclaim update abc123 --start null
```

## Status Values

Tasks in Reclaim can have the following statuses:

- `SCHEDULED` - Task is scheduled on the calendar
- `IN_PROGRESS` - Task is currently being worked on
- `COMPLETE` - Task is marked complete but still active
- `ARCHIVED` - Task is truly completed (set by `reclaim complete`)

**Note:** The `complete` command sets status to `ARCHIVED`, which represents a truly finished task.

## Time Scheme Aliases

When using `--time-scheme`, you can use aliases instead of scheme IDs:

### Work-related aliases:
- `work`
- `working hours`
- `business hours`

These find schemes containing "work" in their name.

### Personal-related aliases:
- `personal`
- `off hours`
- `private`

These find schemes containing "personal" in their name.

**Example:**
```bash
reclaim create --title "Code review" --duration 2 --time-scheme work
```

## GTD Integration

The CLI supports GTD (Getting Things Done) integration:

### ID Tracking Format
Store Reclaim task IDs in your GTD system (e.g., NEXT.md) using this format:

```
[Reclaim:task_id]
```

**Example:**
```markdown
- [ ] Finish quarterly report [Reclaim:abc123def]
- [ ] Review team PRs [Reclaim:xyz789ghi]
```

This allows you to sync your GTD system with Reclaim tasks.

## Error Handling

### Common errors:

**Missing required fields:**
```
Error: --title is required for create command
```
Solution: Provide the `--title` option.

**Invalid task ID:**
```
Error: Task not found: abc123
```
Solution: Verify the task ID using `reclaim list` or `reclaim list completed`.

**Invalid date format:**
```
Error: Invalid date format
```
Solution: Use `YYYY-MM-DD` or `YYYY-MM-DDTHH:MM:SS` format.

**Invalid priority:**
```
Error: Priority must be P1, P2, P3, or P4
```
Solution: Use one of the four priority levels.

## Best Practices

### Use priorities wisely
- Reserve `P1` for truly urgent and important tasks
- Use `P2` for important but less urgent work
- Default `P3` works for most regular tasks
- Use `P4` for nice-to-have items

### Set realistic durations
- Be honest about how long tasks take
- Include buffer time for context switching
- Consider using `--split` for longer tasks to allow flexible scheduling

### Use time schemes effectively
- Create separate schemes for work and personal time
- Use schemes to enforce work-life boundaries
- Apply the right scheme to ensure tasks are scheduled appropriately

### Defer vs Start
- Use `--defer` when you want Reclaim to schedule the task flexibly after a date
- Use `--start` when the task MUST happen at a specific time
- Avoid over-using `--start` as it reduces scheduling flexibility

### Task splitting strategy
- Enable `--split` for tasks longer than 2 hours
- Set `--min-chunk` to maintain focus (e.g., 1 hour minimum)
- Set `--max-chunk` to prevent overly long blocks

### Keep tasks actionable
- Use clear, action-oriented titles
- Add context in `--notes` for future reference
- Complete or delete tasks promptly to keep your list current

## Complete Examples

### Complex task creation
```bash
reclaim create \
  --title "Q4 Planning Session" \
  --due 2025-11-30 \
  --priority P2 \
  --duration 6 \
  --split \
  --min-chunk 1.5 \
  --max-chunk 3 \
  --time-scheme work \
  --category "Planning" \
  --color blue \
  --notes "Include team input from retrospective"
```

### Full task update
```bash
reclaim update abc123 \
  --title "Updated: Q4 Planning Session" \
  --priority P1 \
  --due 2025-11-25 \
  --duration 4 \
  --notes "Deadline moved up due to executive meeting"
```

### Task with deferred start and specific duration
```bash
reclaim create \
  --title "Annual review preparation" \
  --defer 2025-12-01 \
  --duration 3 \
  --priority P2 \
  --time-scheme personal \
  --private true
```
