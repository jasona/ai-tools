---
name: execute
description: "Phase 3: Execute tasks one-by-one from a task list. Requires manual invocation for safety - will not auto-trigger."
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, WebSearch, WebFetch
disable-model-invocation: true
argument-hint: [path to tasks file]
---

## What You're Doing

You're guiding the user through **Phase 3: Execute** — implementing tasks from a task list, **one sub-task at a time**, pausing for approval between each.

**SAFETY NOTE:** This skill requires explicit invocation (`/execute`) because it writes code and may push to git. It will not auto-trigger.

## User's Request

$ARGUMENTS

If no path was provided:
1. Check `./assets/tasks-*.md`.
2. If files exist, list them and ask which one to use.
3. If none exist, say: *"No task files found. Use the `tasks` skill to generate one first."*

## Standards (Baked In)

Apply these rules throughout. They mirror the `execute-tasks` phase of `standards/standards-manifest.yml`.

- **[PRIN-1] User-First** — implementation must serve user needs
- **[PRIN-2] Quality Over Speed** — don't cut corners
- **[PRIN-4] Maintainability** — write code the next engineer can read
- **[SEC-1] No Secrets** — use environment variables; never commit credentials
- **[SEC-2] PII Protection** — no PII in logs or error messages
- **[SEC-3] Input Validation** — validate all user input at boundaries
- **[A11Y]** — UI work must meet the organization's accessibility baseline (WCAG 2.x AA unless otherwise specified)
- **[CODE-ARCH]** — follow existing patterns in the codebase; don't introduce new abstractions without a reason

Note applied standards and any deviations in the commit message or PR description when a parent task completes.

## Execution Protocol

### Rule 1 — One Sub-Task at a Time

**CRITICAL.** Complete ONE sub-task, then STOP and ask for permission before starting the next. Do not batch.

### Rule 2 — Completion Protocol

After finishing a **sub-task**:
1. Change `- [ ]` → `- [x]` in the task file.
2. Save the task file.

When **all sub-tasks** under a parent are `[x]`:
1. Mark the **parent** `[x]`.
2. Commit and push (see *Git Protocol* below). Assume git is initialized.

### Rule 3 — Keep the Task File Current

- Update after every sub-task, not just at parent boundaries.
- Add newly discovered tasks as they emerge.
- Keep the **Relevant Files** section accurate with a one-line purpose per file.

## Process

### 1. Load the Task File

Read the specified task file from `./assets/`.

### 2. Find the Next Sub-Task

Locate the first uncompleted `- [ ]`. If none remain, say: *"All tasks complete. Consider opening a PR."*

### 3. Execute

1. Announce which sub-task you're starting.
2. Implement it following the standards above.
3. Add or update tests where applicable.
4. Mark the sub-task `[x]` and save the task file.
5. Update **Relevant Files** if new files were touched.

### 4. Request Permission

Say:

> *"Completed: [sub-task].*
> *Next up: [next sub-task].*
> *Continue? (y/n)"*

### 5. Wait

**STOP.** Only proceed on `y` / `yes`. On `n`, ask how to adjust.

### 6. Repeat

Return to Step 2.

## Git Protocol

When a **parent task** completes:

1. `git add [specific files]` — prefer named files over `git add -A`.
2. Commit with a descriptive message.
3. Push to the feature branch.

Commit message format:

```
[Feature] Brief description

- What was implemented
- Notable decisions / trade-offs
- Standards applied: v[manifest version], [list]

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Pre-Completion Checklist (per sub-task)

- [ ] Follows existing patterns in the codebase
- [ ] No hardcoded secrets or credentials
- [ ] Inputs validated at boundaries
- [ ] Error handling is appropriate for the context
- [ ] Tests pass where applicable
- [ ] No stray `console.log` / debug code
- [ ] Accessibility baseline met for UI work

## What NOT to Do

- Do NOT complete multiple sub-tasks without explicit approval each time
- Do NOT skip the confirmation step
- Do NOT leave sub-tasks partially implemented
- Do NOT commit secrets, credentials, or PII
- Do NOT bypass failing tests
- Do NOT make changes outside the current sub-task's scope
