---
name: close-task
description: Close a task — finalizes task document, updates changelog, task tracker, and roadmap, runs any project-specific close actions defined in CLAUDE.md
argument-hint: [task name or folder]
---

# /close-task — Close a Task

You are closing a task. The user may name a specific task, or if there's only one in-progress task, close that one.

**Arguments:** $ARGUMENTS

---

## Step 1 — Identify the Task

Determine which task to close:
- If the user named a task: find it
- If there's exactly one in-progress task: confirm with the user that's the one
- If there are multiple in-progress tasks and the user didn't specify: ask which one

Read the task's main document (TASK.md or equivalent) to understand what was done.

---

## Step 2 — Finalize Task Governance

Before closing, ensure the task document is complete:

1. **Status** — Update to Complete / Deployed / Done (use project's convention)
2. **Changes Made** — Is the list of changes accurate and complete? If work was done this session that isn't captured, add it now.
3. **Spec vs. Reality** — Does the spec still match what was actually built? If scope changed during execution, update the spec to reflect what was delivered, not what was originally planned.
4. **Open Questions** — Are there any unresolved items? If so, either resolve them or explicitly note them as deferred/out-of-scope.

---

## Step 3 — Project-Specific Close Actions

Read `CLAUDE.md` for any project-specific task closing procedures. These vary by project and may include:
- Generating change control documentation
- Posting to external systems (Asana, Notion, Slack, etc.)
- Creating deployment checklists
- Writing handoff notes
- Generating PR descriptions

**Follow the project's close procedure exactly.** The skill defines the universal steps; `CLAUDE.md` defines project-specific ones.

If CLAUDE.md has no task-close procedure, that's fine — proceed with the standard steps below.

---

## Step 4 — Standard Close Actions (Always Required)

These happen for every task close, regardless of project:

### 4a — Update Changelog

Find the project's changelog (`CHANGELOG.md` or equivalent). Add an entry for the completed work:
- Follow the existing changelog format and versioning convention
- If no changelog exists, create one with a simple format:
  ```markdown
  # Changelog

  ## [Date] — Task Title
  - What was done (bullet points)
  ```

### 4b — Update Task Tracker

Find the project's task tracker (`TASKS.md` or equivalent):
- Move the task from Active/In Progress to Complete/Deployed
- Update the status indicator
- Add any final notes (e.g., "deployed to prod" or "merged in PR #42")

### 4c — Update Roadmap

If the project has a roadmap (`ROADMAP.md` or equivalent):
- Check off completed items related to this task
- Update the status description if this task was tracked there
- If this task completion unblocks other roadmap items, note that

### 4d — Archive Task Folder

"Archiving" means the task is complete and its folder is now a historical record:
- The task folder stays where it is (don't move or delete it)
- The task document's status is marked complete
- No further updates are expected

The task tracker is what distinguishes active from archived — moving the entry to the "Completed" section is the archive action.

---

## Step 5 — Verify

Do a quick scan to confirm nothing was missed:
- [ ] Task document finalized with accurate status and changes
- [ ] Changelog updated
- [ ] Task tracker updated (moved to completed)
- [ ] Roadmap updated (if applicable)
- [ ] Project-specific close actions completed (per CLAUDE.md)

---

## Step 6 — Report

Tell the user:
- Which task was closed
- What governance files were updated
- Any items that were deferred or flagged as follow-up
- What the next open task or priority is (if known)

Keep it brief — one short paragraph or a bullet list. The files are the record.
