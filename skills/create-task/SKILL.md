---
name: create-task
description: Start a new task — creates task folder, stages governance files, updates task tracker, and verifies existing in-progress tasks are clean before proceeding
argument-hint: <task description or reference>
---

# /create-task — Start a New Task

You are creating a new task. The user may provide a description, reference something from memory, or give a vague idea that needs scoping.

**Arguments:** $ARGUMENTS

---

## Step 1 — Understand the Request

Parse the user's input. It could be:
- A direct description: `/create-task analyze the output of 29 files`
- A reference to something known: `/create-task start the blast radius task`
- A vague idea that needs clarification

If the request is ambiguous, ask one clarifying question before proceeding. Don't over-ask — make reasonable assumptions and state them.

If the user referenced something from memory, a roadmap, or a backlog — find and read the relevant context to pre-populate the task spec.

---

## Step 2 — Check for In-Progress Tasks

**This is critical.** Before creating a new task, check for any currently in-progress tasks.

1. Find the project's task tracker. Look for (in order):
   - `TASKS.md` in the project root
   - A task index section in `CHANGELOG.md`
   - A `tasks/` or `task/` directory with subfolders
   - Any file that serves as a task registry (check `CLAUDE.md` for guidance)

2. If there are tasks marked as "In Progress" or equivalent:
   - Read each in-progress task's main document (TASK.md or equivalent)
   - **Verify its governance is clean:** Is the status accurate? Are recent decisions captured? Is the spec up to date?
   - If anything is stale or incomplete, **fix it now** before moving on. Tell the user what you updated and why.
   - Do NOT silently skip this step — governance hygiene on existing tasks is a prerequisite for opening new ones.

---

## Step 3 — Discover Project Conventions

Read `CLAUDE.md` (or equivalent project instructions) to understand:
- **Task folder structure** — Where do tasks live? What's the naming convention? (e.g., `tasks/YYYY-MM-DD_short-description/`)
- **Required files per task** — What does a task folder need? (e.g., TASK.md, CHANGE_CONTROL.md)
- **Task template** — Is there a defined template for the task document?
- **Task tracker** — What file tracks open/closed tasks?

If the project has no task conventions defined:
1. Propose the following default structure and confirm with the user:
   ```
   tasks/
   └── YYYY-MM-DD_short-description/
       └── TASK.md
   ```
2. Create a `TASKS.md` at the project root as the task index
3. Add a brief "Task Conventions" section to `CLAUDE.md` (or create one if it doesn't exist) documenting the chosen structure

---

## Step 4 — Create the Task

1. **Create the task folder** using the project's naming convention
2. **Create the task document** (TASK.md or equivalent) with:
   - Title
   - Summary (what and why)
   - Spec (what was requested, constraints, decisions)
   - Status: In Progress
   - Any pre-populated context from Step 1

   Use the project's template if one exists. If not, use this minimal default:

   ```markdown
   # Task: [Title]

   ## Summary
   [One paragraph — what problem does this solve and why]

   ## Spec
   [What was requested, constraints, open questions]

   ## Changes Made
   [To be updated as work proceeds]

   ## Status
   🔵 In Progress

   ## Notes
   [Decisions, blockers, discoveries — update as you go]
   ```

3. **Create any additional required files** per project conventions (change control docs, discovery files, etc.)

---

## Step 5 — Update the Task Tracker

Add the new task to the project's task tracker with:
- Folder/path reference
- Title
- Status: In Progress
- Brief description

If no task tracker exists yet, create `TASKS.md`:

```markdown
# TASKS.md — Task Index

## Active

| Folder | Title | Status | Notes |
|--------|-------|--------|-------|
| `tasks/YYYY-MM-DD_description/` | Title | 🔵 In Progress | Brief note |

## Completed

| Folder | Title | Status | Notes |
|--------|-------|--------|-------|
```

---

## Step 6 — Confirm

Tell the user:
- What task was created and where
- What governance cleanup (if any) was done on existing tasks
- What the task's initial scope/spec looks like
- Ask if anything needs adjustment before starting work

Keep it brief. The task folder is the record — don't restate everything you just wrote.
