---
name: list-tasks
description: List tasks by status — shows open, completed, stalled, or all tasks from the project's task tracker
argument-hint: [open|completed|stalled|all]
---

# /list-tasks — List Tasks by Status

Show the user a summary of tasks, optionally filtered by status.

**Arguments:** $ARGUMENTS

---

## Step 1 — Parse Filter

The user may have specified a filter:
- `/list-tasks` — show all tasks, grouped by status
- `/list-tasks open` or `/list-tasks active` — only in-progress tasks
- `/list-tasks completed` or `/list-tasks done` or `/list-tasks closed` — only completed tasks
- `/list-tasks stalled` or `/list-tasks blocked` — tasks that are blocked, stalled, or pending external input
- `/list-tasks all` — everything, including completed

Default (no argument): show **open/active tasks first**, then a count of completed tasks (not the full list).

---

## Step 2 — Find Task Data

Locate the project's task tracking system:

1. **Task tracker file** — Look for `TASKS.md`, a task section in `CHANGELOG.md`, or whatever `CLAUDE.md` specifies
2. **Task folders** — Look for a `tasks/` directory and scan its subfolders
3. **Both** — Cross-reference the tracker against actual folders to catch discrepancies

If the tracker and folders disagree (e.g., a folder exists but isn't in the tracker, or a tracker entry has no folder), flag the discrepancy to the user.

---

## Step 3 — Classify Tasks

Group tasks into these categories:

| Status | Meaning |
|--------|---------|
| 🔵 In Progress | Active work happening |
| ⏳ Pending Review | Built, awaiting feedback or sign-off |
| ⏳ Sandbox Ready | Deployed to sandbox, awaiting prod |
| 🔴 Blocked / Stalled | Cannot proceed — dependency, question, or external blocker |
| 📋 Queued | Defined but not yet started |
| ✅ Complete | Done and closed |

Map the project's status terms to these categories. Different projects use different labels — normalize for display but preserve the original status.

---

## Step 4 — Display

Format the output as a clean table or grouped list. For each task show:
- **Title**
- **Status** (with icon)
- **Location** (folder path or reference)
- **Brief note** (one line — what's the current state or blocker)

Example output:

```
## Active Tasks (3)

| Task | Status | Notes |
|------|--------|-------|
| Regional Rep Goals Dashboard | ⏳ In Team Review | Full dashboard deployed to sandbox |
| C2L FA Link Signing | 🔵 In Progress | Discovery phase — open questions logged |
| FSA Cohort Swap | ⏳ Sandbox Ready | Awaiting team testing for prod |

## Queued (1)
| Flow Blast Radius Map | 📋 Queued | Not yet started |

## Completed: 8 tasks (use `/list-tasks completed` to see all)
```

Keep it scannable. This is a status check, not a deep dive.
