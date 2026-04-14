---
name: tasks
description: "Phase 2: Generate a task list from requirements (PRD, CRD, or DRD). Creates actionable steps for implementation."
allowed-tools: Read, Glob, Grep, Write, Bash(git status, git log, ls)
argument-hint: [path to PRD/CRD/DRD file]
---

## What You're Doing

You're guiding the user through **Phase 2: Generate** — converting a requirements document (PRD, CRD, or DRD) into a structured, checkbox-driven task list that a junior developer can execute.

## User's Request

$ARGUMENTS

If no path was provided:
1. Check `./assets/` for `prd-*.md`, `crd-*.md`, `drd-*.md`.
2. If files exist, list them and ask which one to use.
3. If none exist, say: *"No requirements files found. Use the research / prd / crd / drd skills first, or describe what you want to implement."*

## Standards (Baked In)

Apply these rules throughout. They mirror the `generate-tasks` phase of `standards/standards-manifest.yml`.

- **[PRIN-1] User-First** — tasks must serve requirement intent, not scaffold for its own sake
- **[PRIN-4] Maintainability** — each task should leave the codebase coherent
- **[PRIN-5] Incremental Delivery** — small, individually deliverable sub-tasks

Record applied standards and manifest version (if available) in a small header block at the top of the generated file.

## Process

### 1. Load Requirements

Read the specified requirements file from `./assets/`.

### 2. Analyze

Extract functional requirements, non-functional constraints, technical considerations, and success criteria.

### 3. Phase 1 — Generate Parent Tasks

Create the file with high-level parent tasks only. **Always include `0.0 Create feature branch` first**, unless the user explicitly asks not to branch. Expect roughly 5 additional parents; use judgment.

Present them to the user in this format:

```markdown
## Tasks (High-Level)

- [ ] 0.0 Create feature branch
- [ ] 1.0 [Parent task]
- [ ] 2.0 [Parent task]
- [ ] 3.0 [Parent task]
...
```

Then say, verbatim:

> *"I have generated the high-level tasks based on your requirements. Ready to generate the sub-tasks? Respond with 'Go' to proceed."*

### 4. Wait for Confirmation

**PAUSE.** Do not generate sub-tasks until the user says `Go`.

### 5. Phase 2 — Generate Sub-Tasks

Break each parent into small, specific, actionable sub-tasks:

```markdown
- [ ] 1.0 Parent Task Title
  - [ ] 1.1 [Sub-task]
  - [ ] 1.2 [Sub-task]
```

### 6. Identify Relevant Files

List files likely to be created or modified, including test files.

### 7. Assemble and Save

Final structure:

```markdown
# Tasks: [Feature Name]

Source: `./assets/[requirements-file].md`
Generated: [date]
Standards: v[manifest version if available]

## Relevant Files

- `path/to/file.ts` - Brief description
- `path/to/file.test.ts` - Unit tests for file.ts

### Notes

- Place unit tests alongside the code they test.
- Use `npx jest [optional path]` to run tests (adjust for project tooling).

## Instructions for Completing Tasks

**IMPORTANT:** As each sub-task is finished, change `- [ ]` to `- [x]` in this file. Update after every sub-task, not just after parents.

## Tasks

- [ ] 0.0 Create feature branch
  - [ ] 0.1 Create and checkout `feature/[feature-name]`
- [ ] 1.0 [Parent]
  - [ ] 1.1 [Sub-task]
  - [ ] 1.2 [Sub-task]
- [ ] 2.0 [Parent]
  - [ ] 2.1 [Sub-task]
```

- **Location:** `./assets/`
- **Filename:** `tasks-[feature-name].md`

## Target Audience

A **junior developer** executes this list. Tasks should be specific, logically ordered, and clear about what "done" means.

## What NOT to Do

- Do NOT start implementing tasks (that's the `execute` skill)
- Do NOT skip the "Go" confirmation before expanding sub-tasks
- Do NOT create vague tasks ("improve the code")
- Do NOT forget testing sub-tasks where appropriate
