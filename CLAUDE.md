# CLAUDE.md — claude-task-skills

## Purpose

This repo is a Claude Code plugin that ships six portable task-governance skills. Users install it and get `/create-task`, `/close-task`, `/list-tasks`, `/task-audit`, `/create-task-env`, and `/migrate-tasks` as slash commands in any Claude Code session.

The plugin is intentionally small. The skills are prompts, not code. There are no runtime dependencies, no build step, no tests to run. What matters is that:

1. The marketplace descriptor is valid so `/plugin marketplace add` resolves.
2. The skills agree with each other (same frontmatter schema, same status vocabulary).
3. The skills agree with the sibling project [claude-task-viewer](https://github.com/avanrossum/claude-task-viewer), which reads the files these skills write.

---

## Repo structure

```
claude-task-skills/
├── CLAUDE.md                       # this file
├── TASKS.md                        # task index (rarely active)
├── CHANGELOG.md
├── LESSONS_LEARNED.md
├── README.md                       # user-facing installation + usage
├── LICENSE
├── .claude-plugin/
│   ├── plugin.json                 # plugin manifest
│   └── marketplace.json            # marketplace entry so the repo can be pulled via /plugin marketplace add
└── skills/
    ├── create-task/SKILL.md
    ├── close-task/SKILL.md
    ├── list-tasks/SKILL.md
    ├── task-audit/SKILL.md
    ├── create-task-env/SKILL.md
    └── migrate-tasks/SKILL.md
```

## Shared contracts across skills

### Frontmatter schema

Every skill that reads or writes `TASK.md` agrees on this YAML frontmatter:

```yaml
---
status: in_progress     # pending | in_progress | completed | blocked
priority: 1             # integer, 1 is most urgent
gh_issue_ref: owner/repo#123  # optional
---
```

Rules:
- `status` enum is exactly those four values. The status icons in the README (🔵, ⏳, 🔴, 📋, ✅) are presentation, not data. Don't add new enum values without updating every skill that reads them.
- `priority` defaults differ by skill and that is deliberate:
  - `create-task` defaults to `1` for new work (assumes it's important enough to start).
  - `migrate-tasks` defaults to `999` for legacy tasks (they're "unsorted history" until you triage).
- `gh_issue_ref` is optional. Empty and missing mean the same thing.
- Unknown keys must round-trip. Any skill that rewrites frontmatter preserves keys it doesn't understand.

### Status vocabulary

Skills use the status enum internally. User-facing text maps to icons and labels:

| Enum | Icon | Human label |
|------|------|-------------|
| `in_progress` | 🔵 | In Progress |
| `pending` | 📋 | Pending / Queued |
| `blocked` | 🔴 | Blocked |
| `completed` | ✅ | Complete |

The `⏳` icon in `/list-tasks` for "Pending Review" / "Sandbox Ready" is a *display overlay* — it's not a distinct stored status. If a project needs finer-grained state, it lives in the task body (e.g., a `## Status` subsection) or in a custom frontmatter key, not in the `status` enum.

### Task folder conventions

Default is `tasks/YYYY-MM-DD_short-description/TASK.md` at the project root. Projects can override this in their own `CLAUDE.md`, and all skills read the project's conventions before falling back to defaults.

## Plugin configuration

`marketplace.json` uses the GitHub source form:

```json
{
  "source": {
    "source": "github",
    "repo": "avanrossum/claude-task-skills"
  }
}
```

Do **not** add a `$schema` field — there is no resolvable schema URL for marketplace manifests at this time, and adding one that doesn't resolve has historically caused strict-validator rejection.

`plugin.json` carries the authoritative version string. If versions appear in both files, the manifest wins silently.

## Adding a new skill

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter: `name`, `description`, optional `argument-hint`.
2. Follow the conventional document shape: numbered `## Step N — Title` sections, with a short "Philosophy" preamble if the skill benefits from it.
3. If the skill reads or writes `TASK.md` frontmatter, inherit the schema above. Don't invent new enum values.
4. Add an entry to `README.md` under "Skills" with a one-paragraph description and an example invocation.
5. Append a bullet to `CHANGELOG.md` under the current version's "Skills" section (or bump to a new minor version if releasing).
6. Test by pointing a dev Claude session at the repo via `claude --plugin-dir .` and invoking the command.

## Known not-yet-done

- No CI check that `marketplace.json` validates against Claude Code's live expectations. If the marketplace contract changes, we find out the hard way.
- No automated lint on skill `SKILL.md` frontmatter.
- No snapshot test ensuring all skills agree on the frontmatter schema.

Those are low-priority but documented here so the next session knows they're intentionally absent rather than overlooked.
