---
name: prd
description: "Phase 1: Create a Product Requirements Document for features, functionality, or technical work. Use after /research for best results."
allowed-tools: Read, Glob, Grep, Write, Bash(git status, ls)
argument-hint: [feature name or path to RSD]
---

## What You're Doing

You're guiding the user through **Phase 1: Create** — producing a Product Requirements Document (PRD) that defines *what* to build and *why*. The PRD must be explicit enough for a junior developer to implement.

## User's Request

$ARGUMENTS

If no input was provided, ask: *"What feature or functionality do you want to document? You can also provide the path to an RSD from the research phase."*

## Standards (Baked In)

Apply these rules throughout. They mirror the `create-prd` phase of `standards/standards-manifest.yml`.

- **[PRIN-1] User-First** — requirements must prioritize user value
- **[PRIN-2] Quality Over Speed** — be thorough; ambiguity is the main failure mode of a PRD
- **[PRIN-4] Maintainability** — requirements should enable maintainable solutions
- **[PRIN-5] Incremental Delivery** — large features are broken into deliverable increments
- **[SEC-1] No Secrets** — never reference actual credentials
- **[SEC-3] Input Validation** — note where input validation is required
- **[TERMINOLOGY]** — use the organization's approved product/user terminology consistently
- **[CODE-ARCH]** — prefer integration with existing modules/abstractions over new ones; call this out in *Technical Considerations*

Record a **Standards Compliance** section at the end of the PRD with the manifest version (if available), applied standards, and any deviations with rationale.

## Process

### 1. Check for Existing Research

Look for relevant `./assets/rsd-*.md` files and, if present, use them as the primary input. Also scan for prior `prd-[same-feature]-v*.md` so you can bump the version rather than overwrite.

### 2. Ask Clarifying Questions

Ask only **3–5 critical questions** — only those not reasonably inferable from the prompt or the RSD. Format:
- **Numbered** (1, 2, 3, …)
- **Multiple choice** with options labeled **A, B, C, D**

Typical gaps to probe:
- **Problem / Goal** — what user problem does this solve?
- **Core Functionality** — what key actions can users perform?
- **Scope / Boundaries** — what should this *not* do?
- **Success Criteria** — how will we know it worked?

### 3. Wait for Responses

**PAUSE.** Do not generate the PRD until the user answers.

### 4. Generate the PRD

```markdown
# Product Requirements Document: [Feature Name] v[version]

## 1. Introduction / Overview
Brief description of the feature and the problem it solves. State the goal.

## 2. Goals
Specific, measurable objectives.

## 3. User Stories
- As a [user type], I want to [action] so that [benefit].

## 4. Functional Requirements
Numbered, explicit, unambiguous:
1. The system must …
2. The system must …

## 5. Non-Goals (Out of Scope)
What this feature will NOT include.

## 6. Design Considerations
UI/UX requirements, links to mockups, relevant components/styles.

## 7. Technical Considerations
Known constraints, dependencies, integration points (e.g., "must integrate with Auth module").

## 8. Success Metrics
How success will be measured, with targets.

## 9. Open Questions
Remaining questions / areas needing clarification.

## 10. Standards Compliance
- Manifest version (if available)
- Applied standards
- Deviations with rationale
```

### 5. Save the PRD

- **Location:** `./assets/`
- **Filename:** `prd-[feature-name]-v[version].md`
- Create `./assets/` if missing.
- **NEVER** re-write, revise, or overwrite a file prefaced `prd-`. Always bump the version.

## Target Audience

The PRD is read by a **junior developer**. Be explicit and unambiguous; avoid jargon; include enough detail to convey purpose and core logic without prescribing implementation.

## What NOT to Do

- Do NOT start implementing the feature
- Do NOT skip clarifying questions
- Do NOT overwrite existing `prd-*` files — bump the version
- Do NOT include actual secrets or credentials
