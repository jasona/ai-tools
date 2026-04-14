# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This is **not an application codebase**. It is a collection of Markdown **prompt templates** that guide AI assistants through a structured **Research → Create → Generate → Execute** workflow for software, content, and design projects. There is no build, lint, or test tooling — work here is editing Markdown prompt files and the YAML/Markdown files under `standards/`.

## The workflow the prompts implement

The six top-level `.md` files are designed to be fed to an AI in sequence. Each phase produces a versioned artifact written to a `tasks/` directory (created at runtime, not checked in):

| Phase | Prompt file | Artifact |
|-------|-------------|----------|
| 0. Research | `research.md` | `tasks/rsd-[name]-v[n].md` |
| 1a. Product requirements | `create-prd.md` | `tasks/prd-[name]-v[n].md` |
| 1b. Content requirements | `create-crd.md` | `tasks/crd-[name]-v[n].md` |
| 1c. Design requirements | `create-drd.md` | `tasks/drd-[name]-v[n].md` |
| 2. Task generation | `generate-tasks.md` | `tasks/tasks-[name].md` |
| 3. Execution | `execute-tasks.md` | updates the task list in place |

The execution phase enforces a **one-sub-task-at-a-time** interaction loop: mark `[x]`, stop, wait for user "y"/"yes", then continue. When a parent task becomes fully `[x]`, commit and push. This pattern is core — do not remove it when editing `execute-tasks.md`.

## Corporate Standards system

Every prompt file begins with a "Corporate Standards" preamble that instructs the AI to load `standards/standards-manifest.yml` and then read only the standards files listed under `phases.<phase-name>.includes`. The manifest is the **single source of truth** for which standards apply where — when adding a new standard file under `standards/`, wire it in via the manifest rather than by editing the prompt files.

Structure:
- `standards/global/` — apply to multiple phases (principles, security/privacy, accessibility, terminology)
- `standards/domains/` — domain-specific (code-architecture, content-voice, design-ui)
- `standards/phases/` — per-phase rules, named to match the prompt (`research.md`, `create-prd.md`, etc.)
- `standards/teams/` — optional overlays referenced via `teams:` in the manifest

When the manifest version changes, AI outputs are expected to record the version and list of applied standards in their compliance footer / PR description.

## Editing conventions

- **Keep the standards-manifest in sync.** Adding a standards file without referencing it in the manifest means no prompt will ever load it.
- **Versioned artifacts.** The prompts rely on the `-v1`, `-v2` suffix pattern for iteration; preserve this when editing generation instructions.
- **Mirror changes across prompts.** The four `create-*.md` files and the two task-phase files share structural conventions (clarifying questions, standards preamble, output-path format). When changing one, check whether the others need the same change for consistency.
- **`tasks/` is runtime output**, not source. Do not commit generated RSD/PRD/CRD/DRD/task files unless explicitly asked.

## Common operations

There are no build/test commands. Typical work in this repo:
- Refining prompt wording in the top-level `*.md` files
- Adding or editing standards under `standards/` and registering them in `standards-manifest.yml`
- Bumping `version` / `last_updated` in the manifest when standards change meaningfully
