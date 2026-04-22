---
name: migrate-tasks
description: Add YAML frontmatter to existing TASK.md files — ensures all tasks have required metadata for claude-task-viewer
argument-hint: (no arguments needed)
---

# /migrate-tasks — Add Frontmatter to Existing Tasks

You are migrating task files to include YAML frontmatter. This is a one-time operation per project that ensures all TASK.md files have the metadata structure required by claude-task-viewer.

**Frontmatter schema:**
```yaml
---
status: pending              # pending, in_progress, completed, or blocked
priority: 999               # 1 (highest) → N; use 999 as default if unclear
gh_issue_ref:               # optional: owner/repo#123
---
```

---

## Step 1 — Locate Task Folders

Find all task folders in the project:
1. Look for a `tasks/` directory at the project root
2. Scan for subdirectories matching the naming pattern `YYYY-MM-DD_*` or equivalent (check `CLAUDE.md` for project conventions)
3. Each subfolder should contain at least a `TASK.md`

If you find tasks but no consistent naming pattern, ask the user for clarification before proceeding.

---

## Step 2 — Check Existing Frontmatter

For each task folder:
1. Read the `TASK.md` file
2. Check if it already has frontmatter (starts with `---\n`)
3. If it has frontmatter, skip that task — no action needed
4. If it doesn't, proceed to Step 3

---

## Step 3 — Extract Metadata from Task Content

For tasks without frontmatter, infer reasonable defaults from the TASK.md content:

**Status:**
- Look for a "Status" or "## Status" section in the markdown
- Parse status indicators (🔵 In Progress → `in_progress`, ✅ Complete → `completed`, etc.)
- If no status found, default to `pending`

**Priority:**
- Tasks rarely have explicit priority set. Default to `999` (lowest priority, will be ordered later)
- If the user has notes about relative priority, feel free to adjust, but don't guess

**GitHub Issue Reference:**
- Look for any mention of GitHub issues (e.g., "resolves #123" or "fixes avanrossum/repo#42")
- If found, add as `gh_issue_ref: owner/repo#123`
- If not found, leave blank

---

## Step 4 — Build Frontmatter and Update File

For each task that needs migration:
1. Create frontmatter with the values from Step 3:
   ```yaml
   ---
   status: <status>
   priority: <priority>
   gh_issue_ref: <ref or leave blank>
   ---
   ```

2. Preserve the entire original TASK.md content unchanged (everything after `---`)

3. Write the updated file back to disk

4. Log each migration: "✓ Migrated tasks/YYYY-MM-DD_task-name/"

---

## Step 5 — Confirm and Report

After all migrations:
1. Count total tasks processed
2. Show the user a summary:
   - Total tasks scanned
   - Tasks already had frontmatter (skipped)
   - Tasks migrated
   - Any tasks that had errors
3. Ask if they want to verify any specific task (offer to show a sample frontmatter)

---

## Important Notes

- **Non-destructive:** This only adds frontmatter — no existing content is changed
- **Idempotent:** Safe to run multiple times; already-migrated tasks are skipped
- **Reversible:** If something goes wrong, the change is just removing the first 4 lines of each file
- **Commit the changes:** After migration is complete, ask the user to commit the changes: `git add -A && git commit -m "chore(tasks): add frontmatter to existing TASK.md files"`

---

## Error Handling

If you encounter problems:
- **Can't read a TASK.md**: Skip it, log the error, and continue
- **Malformed markdown**: Still add frontmatter with sensible defaults; no parsing errors should stop the migration
- **TASK.md doesn't exist in a task folder**: Skip that folder and note it

Let the user know about any skipped or problematic tasks after the run.
