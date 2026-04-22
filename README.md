# claude-task-skills

Portable task governance skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Adds structured, folder-based task tracking to any project — create tasks, close them cleanly, audit open work, and manage session lifecycle with consistent governance.

These skills are **project-agnostic**. They discover your project's conventions at runtime by reading `CLAUDE.md`, scanning for task trackers, and adapting. They work out of the box with zero configuration, and layer on top of whatever project-specific behaviors you define in your own `CLAUDE.md`.

## Installation

> All `/plugin` commands below are run **inside a Claude Code session** (CLI, desktop app, or IDE extension). They are Claude Code slash commands, not terminal commands.

### From GitHub

Open Claude Code and run:

```
/plugin marketplace add avanrossum/claude-task-skills
/plugin install claude-task-skills@claude-task-skills
```

The `@claude-task-skills` suffix is the marketplace name (set in `.claude-plugin/marketplace.json`). The plugin and the marketplace happen to share that name here, which looks repetitive but is correct.

### From a local clone

Clone the repo, then start Claude Code with the plugin loaded:

```bash
git clone https://github.com/avanrossum/claude-task-skills.git
claude --plugin-dir ./claude-task-skills
```

### For development / testing

```bash
claude --plugin-dir /path/to/claude-task-skills
```

Reload after making changes (inside a Claude Code session):
```
/reload-plugins
```

## Skills

### `/create-task-env` — Bootstrap Task Governance

One-time setup for projects that don't have task infrastructure yet. Run this first — then all the other task skills work automatically.

```
/create-task-env
```

**What it creates (skipping anything that already exists):**
- `tasks/` directory for task folders
- `TASKS.md` task tracker at the project root
- `CHANGELOG.md` for change history
- Task conventions section appended to `CLAUDE.md` (or creates one)
- Optionally: `ROADMAP.md` and `LESSONS_LEARNED.md` (asks first)

Non-destructive — safe to run on a project that already has partial governance. It fills gaps without overwriting.

---

### `/create-task` — Start a New Task

Creates a task folder, stages governance files, updates the task tracker, and — critically — **verifies that any existing in-progress tasks have clean governance** before opening a new one.

```
/create-task build a CSV export feature for the dashboard
/create-task start the blast radius analysis from the roadmap
```

**What it does:**
1. Parses your description (or looks up a reference from your roadmap/backlog)
2. Checks all in-progress tasks for governance hygiene — fixes stale docs before proceeding
3. Discovers your project's task conventions from `CLAUDE.md` (or proposes sensible defaults)
4. Creates the task folder and stages a TASK.md with title, summary, spec, and status
5. Adds a "task open" entry to your task tracker (`TASKS.md` or equivalent)

---

### `/close-task` — Close a Task

Finalizes a task and updates all governance surfaces. Project-specific close actions (change control docs, Asana posts, Notion updates, etc.) are defined in your `CLAUDE.md` — the skill handles the universal steps.

```
/close-task
/close-task regional rep goals dashboard
```

**Standard actions (always happen):**
- Finalizes the task document (status, changes made, spec accuracy)
- Updates the changelog
- Moves the task to "Completed" in the task tracker
- Updates the roadmap (checks off completed items)
- Runs any project-specific close procedure from `CLAUDE.md`

---

### `/list-tasks` — List Tasks by Status

Quick status check across all tasks. Filters by status, cross-references the tracker against actual task folders, and flags discrepancies.

```
/list-tasks            # Open tasks + completed count
/list-tasks open       # Only in-progress tasks
/list-tasks completed  # Only completed tasks
/list-tasks stalled    # Blocked or stalled tasks
/list-tasks all        # Everything
```

---

### `/task-audit` — Progressive Task Audit

A collaborative, one-at-a-time walk-through of all open tasks. The agent does a silent governance pre-pass first, then walks you through each task that needs attention — surfacing stale status, missing docs, scope creep, and unresolved blockers.

```
/task-audit
```

**How it works:**
1. Silent scan of all open tasks, noting governance issues
2. Opening summary: "Found N open tasks. X healthy, Y need attention, Z may be stale."
3. Progressive walk-through — one task at a time, starting with problems. Waits for your input before moving on.
4. Quick confirmation pass on healthy tasks
5. Wrap-up with summary of changes made

## How It Works

These skills are designed around a simple principle: **`CLAUDE.md` is the extension point**.

The skills define universal task governance behaviors (create folders, update trackers, maintain changelogs). Your project's `CLAUDE.md` defines project-specific behaviors (change control formats, external system integrations, deployment checklists, naming conventions).

### With no project configuration

If your project has no `CLAUDE.md` or task conventions, the skills propose sensible defaults:
- Task folders at `tasks/YYYY-MM-DD_short-description/`
- A `TASK.md` per task with title, summary, spec, status, and notes
- A `TASKS.md` at the project root as the task index

### With project configuration

If your project has a `CLAUDE.md` with task conventions, the skills follow those exactly — folder naming, required files, templates, tracker format, close procedures, and all.

## Task Folder Structure

The default (override via `CLAUDE.md`):

```
your-project/
├── CLAUDE.md           # Project conventions (optional but recommended)
├── TASKS.md            # Task index — open, completed, blocked
├── CHANGELOG.md        # What changed and when
├── ROADMAP.md          # Where we're going (optional)
└── tasks/
    ├── 2026-03-15_csv-export/
    │   └── TASK.md
    ├── 2026-03-20_auth-redesign/
    │   └── TASK.md
    └── ...
```

## Status Icons

| Icon | Meaning |
|------|---------|
| 🔵 | In Progress — active work |
| ⏳ | Pending Review / Sandbox Ready — waiting on someone else |
| 🔴 | Blocked / Stalled — cannot proceed |
| 📋 | Queued — defined but not started |
| ✅ | Complete |

## License

MIT
