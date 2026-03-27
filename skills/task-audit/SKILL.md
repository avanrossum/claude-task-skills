---
name: task-audit
description: Progressive task audit — walks through each open task with the user, checking governance health, stale status, and missing documentation
argument-hint: ""
---

# /task-audit — Progressive Task Audit

You are walking the user through a structured audit of all open tasks. This is a collaborative, progressive process — not a dump of information.

**Arguments:** $ARGUMENTS

---

## Philosophy

A task audit is a **conversation**, not a report. The goal is to surface stale tasks, update governance, resolve ambiguity, and help the user decide what to prioritize. You lead the process; the user makes decisions.

---

## Step 1 — Silent Governance Pre-Pass

Before involving the user, do a quick scan of all open tasks **silently**:

1. Find the task tracker (`TASKS.md` or equivalent) and read it
2. For each open/in-progress task, read its task document (TASK.md or equivalent)
3. Note any issues you find — but don't fix them yet. Flag them for discussion:
   - **Stale status** — task says "In Progress" but no changes in weeks
   - **Missing governance** — task folder exists but TASK.md is incomplete or missing
   - **Orphaned entries** — tracker references a task that doesn't exist, or a folder exists without a tracker entry
   - **Ambiguous state** — status is unclear, or the task document contradicts the tracker
   - **Scope creep** — spec says one thing but changes made suggest a different scope
   - **Unresolved blockers** — task says "blocked" with no plan to unblock
   - **Governance debt** — decisions made but not documented, open questions with no owner

---

## Step 2 — Opening Summary

Present a brief overview to the user:

```
Found N open tasks. Quick health check:
- X tasks look healthy (governance current, status accurate)
- Y tasks have issues I want to walk through with you
- Z tasks may be stale or ready to close
```

Then ask: "Ready to walk through them? I'll go one at a time, starting with the ones that need attention."

---

## Step 3 — Progressive Walk-Through

Go through tasks **one at a time**, starting with the ones that have issues (from Step 1). For each task:

### Present
- Task name and current status
- One-line summary of what it is
- Any issues found in the pre-pass

### Ask
Depending on the issues, ask the relevant questions:
- "This has been in progress since [date] with no recent updates. Is it still active, or should we mark it stalled/closed?"
- "The spec says X but the changes suggest Y. Which is accurate?"
- "This is marked blocked. What's the blocker and is there a path to unblock?"
- "This looks done. Should we close it?"
- "The task document is missing [section]. Want me to fill it in based on what I can see?"

### Act
After the user responds:
- Update the task document immediately
- Update the task tracker if status changed
- Note any follow-up items

Then move to the next task. **Wait for the user's response before proceeding** — don't batch multiple tasks into one message.

---

## Step 4 — Healthy Tasks (Quick Confirmation)

After addressing problem tasks, briefly run through the healthy ones:

"These N tasks look current and healthy — just confirming:"
- Task A — [status] — still accurate?
- Task B — [status] — still accurate?

The user can confirm all at once or flag any that need discussion.

---

## Step 5 — Wrap-Up

After all tasks are reviewed:

1. Summarize what changed:
   - Tasks closed: N
   - Tasks updated: N
   - New blockers identified: N
   - Tasks still healthy: N

2. Update the task tracker to reflect all changes made during the audit

3. If appropriate, suggest priority order for remaining open tasks

Keep the wrap-up brief. The value was in the conversation, not the summary.
