# reclaim-skills

A Claude Code plugin marketplace with one plugin: **reclaim**, which manages your [Reclaim.ai](https://reclaim.ai) tasks from inside a Claude Code session — create, update, list, complete, and delete, with a confirmation step before anything gets written.

## Installation

```sh
claude plugin marketplace add benjaminjackson/reclaim-skills
claude plugin install reclaim@reclaim-skills
```

The skill drives the [`reclaim` CLI](https://rubygems.org/gems/reclaim) (`gem install reclaim`), but you don't have to install it up front — if a command fails because the CLI is missing, the skill installs the gem itself and only asks you to step in if that fails (say, because Ruby isn't there).

## reclaim

One skill: **reclaim-tasks**, full CRUD for Reclaim.ai tasks via the `reclaim` CLI.

### reclaim-tasks

Handles the whole task lifecycle: creating tasks with a title, due date, priority (P1–P4), duration, notes, deferral or start dates, splitting into chunks, and a time scheme (work or personal hours); updating any of those on an existing task; listing active, completed, or overdue tasks; fetching a task's details; listing your available time schemes; and marking tasks complete or deleting them.

The safety rule is the heart of it: every write — create, update, complete, delete — shows you the exact command it's about to run and waits for your approval before executing. Reads (list, get, list-schemes) run immediately, no confirmation needed.

It also knows one genuinely confusing quirk of Reclaim's API: the active-task list shows a COMPLETE status with a checkmark for tasks whose *scheduled time blocks* are in the past, not tasks that are actually done. The skill treats everything in the active list as open work until you explicitly complete it, so a checkmark won't fool it into thinking you finished something you didn't.

#### Usage

- **Automatically:** on requests like "show me my active Reclaim tasks," "create a task called 'Write proposal' due Friday, P1, 3 hours," "change task abc123's due date to Monday," or "mark task abc123 as complete" — anything touching Reclaim tasks, calendar scheduling, priorities, time blocking, or task durations.

## Author

Benjamin Jackson ([@benjaminjackson](https://github.com/benjaminjackson))
