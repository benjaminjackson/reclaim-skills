# Reclaim Skills for Claude Code

Agent Skills for managing tasks in [Reclaim.ai](https://reclaim.ai) using Claude Code.

## Overview

This skill provides comprehensive CRUD operations for Reclaim.ai tasks with a safety-first approach. All write operations (create, update, complete, delete) require explicit user confirmation before execution.

## Features

- **Create tasks** with flexible options (priority, duration, dates, time schemes, splitting)
- **Update tasks** to change any task property
- **List and filter** tasks (active, completed, overdue)
- **Complete tasks** (marks as ARCHIVED)
- **Delete tasks** permanently
- **Query time schemes** to see available scheduling windows
- **Confirmation workflow** for all write operations to prevent accidental changes

## Prerequisites

1. [Claude Code](https://claude.com/claude-code) version 1.0 or later
2. [Reclaim.ai account](https://reclaim.ai)
3. Ruby (for installing the Reclaim gem)

## Installation

### Step 1: Install the Reclaim CLI

Install the Reclaim gem:

```bash
gem install reclaim
```

Verify installation:

```bash
reclaim --help
```

### Step 2: Install the Skill

Install the skill from the Claude Code marketplace:

```
/plugin marketplace add benjaminjackson/reclaim-skills
```

## Verification

Verify the skill is installed by asking Claude Code:

```
What skills are available?
```

You should see `reclaim-tasks` in the list.

## Usage

The skill activates automatically when you mention tasks, Reclaim, or scheduling. You don't need to explicitly invoke it.

### Quick Examples

**List tasks:**
```
Show me my active Reclaim tasks
```

**Create a task:**
```
Create a Reclaim task called "Write proposal" due Friday, P1 priority, 3 hours
```

**Update a task:**
```
Update task abc123 to P2 priority and change due date to next Monday
```

**Complete a task:**
```
Mark task abc123 as complete
```

### Confirmation Workflow

For all write operations (create, update, complete, delete), Claude will:

1. Parse your request
2. Show you the exact command that will be executed
3. Ask for confirmation using a dialog
4. Execute only after you approve

**Example confirmation dialog:**
```
Ready to create this Reclaim task:

Command: reclaim create --title "Write proposal" --due 2025-11-14 --priority P1 --duration 3

This will create:
- Title: "Write proposal"
- Priority: P1
- Due: 2025-11-14
- Duration: 3 hours

Proceed?
```

### Read-only Operations

These execute immediately without confirmation:
- `reclaim list` (or variants: active, completed, overdue)
- `reclaim get TASK_ID`
- `reclaim list-schemes`

## Skill Documentation

The skill includes three documentation files:

- **[SKILL.md](reclaim-tasks/SKILL.md)** - Quick reference and overview
- **[EXAMPLES.md](reclaim-tasks/EXAMPLES.md)** - Comprehensive examples for all workflows
- **[REFERENCE.md](reclaim-tasks/REFERENCE.md)** - Complete option and command reference

Claude will progressively load these as needed to answer your questions.

## Common Workflows

### Morning Planning
```
Show me my overdue tasks
List my active tasks for today
Update task xyz to P1 priority
```

### Quick Task Capture
```
Create a task "Review PR" with 30 minutes duration
Add a task "Team standup" tomorrow at 9am, 1 hour
```

### Task Management
```
Show details for task abc123
Change the due date of task abc123 to next Friday
Mark task abc123 as complete
```

### Advanced Scheduling
```
Create a 6-hour task "Documentation sprint" that can be split into 1.5 hour chunks, P2 priority, work hours only
```

## Task Properties Reference

### Priority Levels
- **P1** - Highest priority (urgent and important)
- **P2** - High priority
- **P3** - Medium priority (default)
- **P4** - Low priority

### Duration Format
Specified in hours (decimal):
- `0.25` = 15 minutes
- `0.5` = 30 minutes
- `1` = 1 hour
- `1.5` = 90 minutes
- `2` = 2 hours
- `4` = 4 hours

### Date Formats
- **Date**: `YYYY-MM-DD` (e.g., `2025-11-15`)
- **DateTime**: `YYYY-MM-DDTHH:MM:SS` (e.g., `2025-11-15T14:30:00`)
- **Clear date**: Use `none`, `clear`, or `null`

### Time Schemes
Use aliases for common schemes:
- `work`, `working hours`, `business hours` → Work time
- `personal`, `off hours`, `private` → Personal time
- Or use specific scheme IDs from `reclaim list-schemes`

## Troubleshooting

### Skill not activating

**Check installation:**
```bash
ls ~/.claude/skills/reclaim-tasks/SKILL.md
```

**Verify YAML frontmatter:**
```bash
head -n 10 ~/.claude/skills/reclaim-tasks/SKILL.md
```

**Restart Claude Code** to reload skills.

### CLI not found

Ensure the `reclaim` CLI is installed and in your PATH:
```bash
which reclaim
reclaim --help
```

### Invalid task ID errors

Use `reclaim list` to verify task IDs before updating/completing/deleting.

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Support

- **Issues**: [GitHub Issues](https://github.com/benjaminjackson/reclaim-skills/issues)
- **Reclaim.ai**: [Reclaim Support](https://reclaim.ai/support)
- **Claude Code**: [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)

## Related Resources

- [Reclaim.ai](https://reclaim.ai) - Smart calendar scheduling
- [Claude Code Skills Guide](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/quickstart)
- [Agent Skills Best Practices](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices)

## Changelog

### v1.0.0 (2025-11-02)
- Initial release
- Complete CRUD operations for Reclaim tasks
- Confirmation workflow for write operations
- Comprehensive documentation and examples
