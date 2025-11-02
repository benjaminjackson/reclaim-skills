# Reclaim Tasks: Examples

Comprehensive examples for all Reclaim task workflows. Remember: All write operations (create, update, complete, delete) require confirmation using AskUserQuestion before execution.

## Quick Task Capture

### Minimal task creation
```bash
reclaim create --title "Review documents"
```

### Task with duration only
```bash
reclaim create --title "Research competitors" --duration 2
```

### Task with due date
```bash
reclaim create --title "Submit report" --due 2025-11-15
```

### Task with priority
```bash
reclaim create --title "Fix critical bug" --priority P1
```

## Detailed Task Creation

### Complete task specification
```bash
reclaim create \
  --title "Write quarterly review" \
  --due 2025-11-30 \
  --priority P2 \
  --duration 4 \
  --time-scheme work \
  --notes "Include metrics from Q3 and Q4"
```

### Task with specific start time
```bash
reclaim create \
  --title "Client presentation" \
  --start 2025-11-10T14:00:00 \
  --duration 1.5 \
  --priority P1
```

### Deferred task (start after specific date)
```bash
reclaim create \
  --title "Plan 2026 roadmap" \
  --defer 2025-12-01 \
  --duration 3 \
  --priority P2
```

### Task with splitting enabled
```bash
# Allow splitting with default minimum chunk
reclaim create \
  --title "Code review backlog" \
  --duration 4 \
  --split

# Allow splitting with 30-minute minimum chunks
reclaim create \
  --title "Email cleanup" \
  --duration 3 \
  --split 0.5

# Splitting with min and max chunk constraints
reclaim create \
  --title "Documentation updates" \
  --duration 6 \
  --split \
  --min-chunk 0.5 \
  --max-chunk 2
```

## Task Updates

### Update task title
```bash
reclaim update abc123 --title "Updated title"
```

### Change priority
```bash
reclaim update abc123 --priority P1
```

### Update due date
```bash
reclaim update abc123 --due 2025-11-20
```

### Update duration
```bash
reclaim update abc123 --duration 2.5
```

### Multiple updates at once
```bash
reclaim update abc123 \
  --title "Refactored title" \
  --priority P2 \
  --duration 3 \
  --due 2025-11-25
```

### Clear a date field
```bash
# Clear due date
reclaim update abc123 --due none

# Clear deferred start
reclaim update abc123 --defer clear

# Clear specific start time
reclaim update abc123 --start null
```

### Add or update notes
```bash
reclaim update abc123 --notes "Updated context and requirements"
```

### Change time scheme
```bash
# Using alias
reclaim update abc123 --time-scheme work

# Using specific scheme ID
reclaim update abc123 --time-scheme ts_abc123def
```

## Listing and Filtering

### List active tasks (default)
```bash
reclaim
# or explicitly
reclaim list active
```

### List completed tasks
```bash
reclaim list completed
```

### List overdue tasks
```bash
reclaim list overdue
```

### Get specific task details
```bash
reclaim get abc123
```

### List available time schemes
```bash
reclaim list-schemes
```

## Task Completion

### Mark task as complete (ARCHIVED status)
```bash
reclaim complete abc123
```

Note: This sets the task to ARCHIVED status, which represents truly complete tasks in Reclaim.

## Task Deletion

### Permanently delete a task
```bash
reclaim delete abc123
```

Warning: This is permanent deletion. Use `complete` instead if you want to mark a task as done.

## Duration Formats

Tasks can have various durations specified in hours:

```bash
# 15 minutes
reclaim create --title "Quick check-in" --duration 0.25

# 30 minutes
reclaim create --title "Review PR" --duration 0.5

# 45 minutes
reclaim create --title "Team standup" --duration 0.75

# 1 hour
reclaim create --title "Deep work session" --duration 1

# 90 minutes
reclaim create --title "Workshop prep" --duration 1.5

# 2 hours
reclaim create --title "Client meeting" --duration 2

# 4 hours (half day)
reclaim create --title "Strategic planning" --duration 4

# 8 hours (full day)
reclaim create --title "Conference attendance" --duration 8
```

## Date and Time Formats

### Date only (YYYY-MM-DD)
```bash
reclaim create --title "Submit proposal" --due 2025-11-30
```

### Date with time (YYYY-MM-DDTHH:MM:SS)
```bash
reclaim create --title "Presentation" --start 2025-11-15T14:30:00
```

### Clearing dates
```bash
# All of these work to clear a date field
reclaim update abc123 --due none
reclaim update abc123 --defer clear
reclaim update abc123 --start null
```

## Workflow Examples

### Morning planning workflow
```bash
# List what's scheduled
reclaim list active

# Check overdue items
reclaim list overdue

# Adjust priorities based on day
reclaim update task1 --priority P1
reclaim update task2 --defer 2025-11-08
```

### Creating a project task series
```bash
# Phase 1: Research (this week)
reclaim create \
  --title "Research: User interviews" \
  --due 2025-11-08 \
  --priority P1 \
  --duration 3 \
  --split 1

# Phase 2: Design (next week)
reclaim create \
  --title "Design: Wireframes" \
  --defer 2025-11-11 \
  --duration 4 \
  --priority P2

# Phase 3: Implementation (following week)
reclaim create \
  --title "Implementation: Core features" \
  --defer 2025-11-18 \
  --duration 8 \
  --priority P2 \
  --split 2
```

### End of day cleanup
```bash
# Complete finished tasks
reclaim complete task1
reclaim complete task2

# Defer tasks that weren't started
reclaim update task3 --defer 2025-11-08

# Review what's coming up
reclaim list active
```

## Time Scheme Examples

### Using work hours
```bash
reclaim create \
  --title "Code review" \
  --duration 2 \
  --time-scheme work
```

### Using personal time
```bash
reclaim create \
  --title "Side project work" \
  --duration 1.5 \
  --time-scheme personal
```

### Finding and using specific schemes
```bash
# List all available schemes
reclaim list-schemes

# Use specific scheme ID
reclaim create \
  --title "Deep focus work" \
  --duration 3 \
  --time-scheme ts_abc123def
```

## Private Tasks

### Create a private task
```bash
reclaim create \
  --title "Personal development review" \
  --duration 1 \
  --private true
```

### Make existing task private
```bash
reclaim update abc123 --private true
```

### Make task non-private
```bash
reclaim update abc123 --private false
```

## Category and Color

### Set event category
```bash
reclaim create \
  --title "Team meeting" \
  --duration 1 \
  --category "Meetings"
```

### Set event color
```bash
reclaim create \
  --title "Deep work block" \
  --duration 2 \
  --color "blue"
```

### Update category and color
```bash
reclaim update abc123 --category "Planning" --color "green"
```

## Confirmation Workflow Example

When a user asks: "Create a task to finish the proposal, P1, due next Friday, 3 hours"

**Step 1**: Construct the command
```bash
reclaim create \
  --title "Finish the proposal" \
  --priority P1 \
  --due 2025-11-14 \
  --duration 3
```

**Step 2**: Use AskUserQuestion with:
```
Ready to create this Reclaim task:

Command: reclaim create --title "Finish the proposal" --priority P1 --due 2025-11-14 --duration 3

This will create:
- Title: "Finish the proposal"
- Priority: P1
- Due: 2025-11-14
- Duration: 3 hours

Proceed?
```

**Step 3**: After user confirms, execute the command
