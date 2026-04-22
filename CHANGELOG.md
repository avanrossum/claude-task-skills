# Changelog

All notable changes to claude-task-skills are documented here. Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/). Versioning: semver.

## [1.1.1] — 2026-04-22

### Fixed

- **`marketplace.json`** rewritten to the canonical GitHub source form: `source: { source: "github", repo: "avanrossum/claude-task-skills" }`. Previous value used `source: "url"` with a `$schema` URL that wasn't a real Anthropic endpoint, which may have been contributing to historical "invalid schema" validation failures.
- **README install command** corrected from `/plugin install claude-task-skills@avanrossum-claude-task-skills` to `/plugin install claude-task-skills@claude-task-skills`. The `@` suffix is the marketplace name (set by the `name` field in `marketplace.json`), not a GitHub-derived slug.
- **`/create-task-env` TASK.md template** finally carries YAML frontmatter (`status` / `priority` / `gh_issue_ref`). Previously, projects bootstrapped with this skill produced task files that `/create-task` and the sibling [claude-task-viewer](https://github.com/avanrossum/claude-task-viewer) couldn't classify.
- **`/create-task-env` CLAUDE.md conventions block** now documents the frontmatter schema it expects downstream.

### Added

- `CLAUDE.md` at the repo root — shared contracts across skills, plugin configuration rules, how to add a new skill.
- `TASKS.md` — task index for this plugin.
- `LESSONS_LEARNED.md` — captured the `$schema` + source-form + skill-consistency lessons so the next session doesn't re-discover them.

### Changed

- `marketplace.json` owner simplified to just `name` (the `email` field was extraneous).

## [1.1.0] — 2026-04-12

### Added

- **`/migrate-tasks`** — Add YAML frontmatter to existing `TASK.md` files. Non-destructive, idempotent. Used once per project to bring pre-frontmatter tasks into the schema that `/create-task`, `/close-task`, `/list-tasks`, and `/task-audit` expect.
- Initial `.claude-plugin/marketplace.json` so the repo can be pulled via `/plugin marketplace add avanrossum/claude-task-skills`.

## [1.0.0] — 2026-03-27

Initial release.

### Skills

- **`/create-task`** — Start a new task with folder, governance, and tracker update. Verifies existing in-progress tasks are clean before proceeding.
- **`/close-task`** — Close a task with changelog, tracker, and roadmap updates. Defers to the project's `CLAUDE.md` for project-specific close actions.
- **`/list-tasks`** — List tasks filtered by status (open, completed, stalled, all). Cross-references the tracker against task folders.
- **`/task-audit`** — Progressive, collaborative audit of all open tasks. Silent governance pre-pass, then one-at-a-time walk-through.
- **`/create-task-env`** — Bootstrap task governance infrastructure for projects that don't have one. Creates tracker, task directory, changelog, and adds conventions to `CLAUDE.md`.

### Infrastructure

- Plugin manifest (`.claude-plugin/plugin.json`).
- MIT license.
- README with installation instructions and full skill documentation.
