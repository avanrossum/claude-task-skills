# LESSONS_LEARNED.md

## Don't invent `$schema` URLs

`marketplace.json` originally carried `"$schema": "https://anthropic.com/claude-code/marketplace.schema.json"`. That URL never existed. In a past session, nine commits went into guessing at the marketplace schema shape, all failing with "Invalid schema" errors. At least some of those failures were plausibly caused by a validator trying to resolve the fake `$schema` URL, not by bad content.

**Rule:** if a docs page doesn't mention a field, don't add it speculatively. For Claude Code plugin manifests, follow https://code.claude.com/docs/en/plugin-marketplaces exactly.

## Marketplace source form

The canonical source form for a GitHub-hosted plugin is:

```json
"source": {
  "source": "github",
  "repo": "owner/repo"
}
```

The `"source": "url"` form with a raw git URL works but is not what the docs lead with, and may not behave identically in every install flow. Default to `github` whenever the plugin lives in a GitHub repo.

## Skills that read the same files must agree on the data shape

The 2026-04-22 audit caught a footgun: `/create-task-env` generated a `TASK.md` template with no frontmatter, while `/create-task` required frontmatter and `claude-task-viewer` (sibling project) needed it to classify tasks. So the user bootstrapped a project with `/create-task-env`, created a task manually using the documented template, and produced a file the rest of the toolchain couldn't read.

**Rule:** treat the task frontmatter schema as a contract. Any skill that writes a template owns a share of that contract. Update all templates together or not at all.

## Priority-default divergence is deliberate

`create-task` defaults `priority: 1`. `migrate-tasks` defaults `priority: 999`. This is intentional — new work starts important; legacy work starts "unsorted." Left undocumented it looks like a bug. Now documented in `CLAUDE.md`, because the next maintainer who sees the divergence will assume inconsistency and "fix" it.

**Rule:** any deliberately divergent default deserves a one-line comment or doc note naming both values and the reason.

## A governance plugin has to practice governance

The repo preached task governance without having any of its own — no `CLAUDE.md`, no `TASKS.md`, no `CHANGELOG.md` that kept up with the skills added after release. Fixed as part of the 2026-04-22 audit. If you're the next session and you catch yourself skipping this repo's own docs because "it's a small plugin," that's exactly the attitude that caused the drift.
