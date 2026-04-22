# TASKS.md — Task Index

This plugin carries very little in-progress work. Most changes are small, self-contained, and ship in a single commit without a formal task folder. When larger changes are warranted (schema migrations, a new skill family, contract shifts), they get a task folder in `tasks/`.

## Active

_None._

## Completed

| Date | Change | Notes |
|------|--------|-------|
| 2026-04-22 | Audit + fix pass | marketplace.json now uses the canonical GitHub source form (was `"source": "url"` with a fictional `$schema`); `create-task-env` TASK.md template finally carries YAML frontmatter; added `CLAUDE.md`, this `TASKS.md`, and `LESSONS_LEARNED.md`; CHANGELOG caught up to the real state. |
| 2026-04-12 | Added `/migrate-tasks` skill + initial marketplace.json | Commit `e1c398d`. |
| 2026-03-27 | Initial release of 5 skills | Commit `de101ea`. |

See `CHANGELOG.md` for the full release-by-release record.
