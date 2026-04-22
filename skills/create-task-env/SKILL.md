---
name: create-task-env
description: Bootstrap task governance infrastructure for a project — creates task tracker, tasks directory, changelog, and adds task conventions to CLAUDE.md
argument-hint: ""
---

# /create-task-env — Bootstrap Task Governance

You are setting up the task governance infrastructure for a project that doesn't have one yet. This is a one-time setup — run it once, and the other task skills (`/create-task`, `/close-task`, `/list-tasks`, `/task-audit`) will work automatically from then on.

**Arguments:** $ARGUMENTS

---

## Philosophy

This skill is **additive and non-destructive**. It adds what's missing without overwriting what exists. If a project already has a CLAUDE.md, it appends to it. If it already has a changelog, it leaves it alone. Run it on a fresh project or a mature one — it adapts.

---

## Step 1 — Assess Current State

Scan the project root for existing governance infrastructure. Check for each of these and note what exists vs. what's missing:

- [ ] `CLAUDE.md` — project instructions for Claude
- [ ] `TASKS.md` — task tracker / index
- [ ] `tasks/` directory — task folder structure
- [ ] `CHANGELOG.md` — change history
- [ ] `ROADMAP.md` — priorities and direction
- [ ] `LESSONS_LEARNED.md` — institutional knowledge

Also check:
- Is this a git repo? (It should be — warn if not)
- Does CLAUDE.md already have task conventions? (If so, skip that section)
- Is there an alternative task tracking system already in place? (e.g., issues, a different file)

---

## Step 2 — Create Missing Infrastructure

For each missing piece, create it. Skip anything that already exists.

### 2a — `tasks/` directory

Create the directory:
```bash
mkdir -p tasks
```

### 2b — `TASKS.md`

Create the task tracker at the project root:

```markdown
# TASKS.md — Task Index

Master index of all task folders under `tasks/`. Update this file when a task is opened, its status changes, or it is closed.

---

## Active / In Progress

| Folder | Title | Status | Notes |
|--------|-------|--------|-------|

---

## Completed

| Folder | Title | Status | Notes |
|--------|-------|--------|-------|

---

## Legend

| Icon | Meaning |
|------|---------|
| 🔵 In Progress | Active work — development ongoing |
| ⏳ Pending Review | Built, awaiting feedback or sign-off |
| 🔴 Blocked / Stalled | Cannot proceed — dependency or external blocker |
| 📋 Queued | Defined but not yet started |
| ✅ Complete | Done and closed |
```

### 2c — `CHANGELOG.md`

```markdown
# Changelog

All notable changes to this project are documented here.

Format: entries grouped by date, with bullet points describing what changed.

---

## [YYYY-MM-DD] — Task Governance Initialized

- Created `TASKS.md` task tracker
- Created `tasks/` directory for task folders
- Added task conventions to `CLAUDE.md`
```

Fill in today's date.

### 2d — `ROADMAP.md` (optional — ask first)

Only create if the user wants one. Some projects don't need a roadmap. Ask:

> "Want me to also create a ROADMAP.md for tracking priorities and milestones? (Not every project needs one — skip if this is a small or short-lived project.)"

If yes:

```markdown
# Roadmap

> Last updated: YYYY-MM-DD

---

## Current Priorities

- [ ] *Add priorities here*

---

## Backlog

- *Add backlog items here*

---

## Completed

- [x] Task governance infrastructure set up
```

### 2e — `LESSONS_LEARNED.md` (optional — ask first)

Same as roadmap — only create if the user wants it.

```markdown
# Lessons Learned

Patterns, gotchas, anti-patterns, and non-obvious findings. Log anything here that would save time if encountered again.

---

*No entries yet.*
```

---

## Step 3 — Update CLAUDE.md

This is the most important step. The task conventions need to live in `CLAUDE.md` so that Claude (in any future session) knows how to work with tasks.

### If CLAUDE.md doesn't exist

Create it with a minimal structure that includes the task conventions:

```markdown
# CLAUDE.md — Project Guidelines

## Task Conventions

Every discrete piece of work gets a folder under `tasks/`.

### Task Folder Structure

```
tasks/YYYY-MM-DD_short-description/
└── TASK.md
```

### TASK.md Template

Each task folder must contain a `TASK.md` with YAML frontmatter. The frontmatter is mandatory — it is what `/list-tasks`, `/task-audit`, and the separate [claude-task-viewer](https://github.com/avanrossum/claude-task-viewer) dashboard all read to classify and filter tasks.

```markdown
---
status: in_progress
priority: 1
gh_issue_ref:
---

# Task: [Title]

## Summary
One paragraph — what problem does this solve and why.

## Spec
What was requested, constraints, open questions.

## Changes Made
Bullet list of every change made during this task.

## Notes
Decisions, blockers, discoveries — update as you go.
```

### Frontmatter schema

- `status` — one of `pending`, `in_progress`, `completed`, `blocked`. The visual icons (🔵, ⏳, 🔴, 📋, ✅) used in `TASKS.md` are surface presentation, not data.
- `priority` — integer. `1` is most urgent. New tasks default to `1`; tasks migrated from pre-frontmatter history default to `999` so they're clearly "unsorted legacy work."
- `gh_issue_ref` — optional, form `owner/repo#number`. Leave blank or omit when not applicable.

Any additional frontmatter keys (e.g., `assignee`, `tags`) are preserved on round-trip.

### Task Lifecycle

- **Opening a task:** Create the folder, write TASK.md, add entry to TASKS.md
- **During work:** Keep TASK.md updated with decisions and changes as they happen
- **Closing a task:** Update TASK.md status, update CHANGELOG.md, move entry in TASKS.md to Completed section, update ROADMAP.md if applicable
- **Task tracker:** `TASKS.md` at the project root is the index of all tasks

### Governance Rules

- Create the task folder **before** starting work
- Keep TASK.md updated as decisions are made — don't reconstruct after the fact
- Update `TASKS.md` whenever a task is opened, its status changes, or it is closed
```

### If CLAUDE.md exists but has no task conventions

Append the task conventions section above to the existing file. Place it logically — after any existing project description, before any appendices or reference sections.

### If CLAUDE.md already has task conventions

Skip this step. Do not overwrite or duplicate existing conventions.

---

## Step 4 — Git Add (but don't commit)

Stage the new files:
```bash
git add TASKS.md CHANGELOG.md tasks/ CLAUDE.md
```

Also stage ROADMAP.md and LESSONS_LEARNED.md if they were created.

**Do not commit.** Tell the user what was staged and let them review before committing. Suggest a commit message:

```
chore: initialize task governance infrastructure
```

---

## Step 5 — Report

Tell the user what was created, what was skipped (already existed), and what they should do next:

```
Task governance initialized:
- Created: TASKS.md, tasks/, CHANGELOG.md
- Updated: CLAUDE.md (added task conventions section)
- Skipped: ROADMAP.md (already exists)

Files are staged but not committed. Review with `git diff --cached` then commit when ready.

You can now use:
- /create-task <description> — to start a new task
- /close-task — to close the current task
- /list-tasks — to see task status
- /task-audit — to review all open tasks
```
