# Changelog

All notable changes to claude-task-skills are documented here.

---

## [1.0.0] — 2026-03-27

Initial release.

### Skills

- **`/create-task`** — Start a new task with folder, governance, and tracker update. Verifies existing in-progress tasks are clean before proceeding.
- **`/close-task`** — Close a task with changelog, tracker, and roadmap updates. Defers to project's CLAUDE.md for project-specific close actions.
- **`/list-tasks`** — List tasks filtered by status (open, completed, stalled, all). Cross-references tracker against task folders.
- **`/task-audit`** — Progressive, collaborative audit of all open tasks. Silent governance pre-pass, then one-at-a-time walk-through.
- **`/create-task-env`** — Bootstrap task governance infrastructure for projects that don't have one. Creates tracker, task directory, changelog, and adds conventions to CLAUDE.md.

### Infrastructure

- Plugin manifest (`.claude-plugin/plugin.json`)
- MIT license
- README with installation instructions and full skill documentation
