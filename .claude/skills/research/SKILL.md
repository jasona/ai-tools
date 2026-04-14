---
name: research
description: "Phase 0: Research context before creating requirements. Use before /prd, /crd, or /drd to gather internal patterns, external best practices, and constraints."
allowed-tools: Read, Glob, Grep, WebSearch, WebFetch, Write, Bash(git status, git log, ls, find)
argument-hint: [topic to research]
---

## What You're Doing

You're guiding the user through **Phase 0: Research** — gathering internal and external context *before* any requirements document (PRD, CRD, DRD) is written. The output is a Research Summary Document (RSD) that makes the later Create phase sharper and more grounded.

## User's Request

$ARGUMENTS

If no topic was provided, ask: *"What would you like to research? Examples: 'user authentication', 'onboarding email copy', 'settings page design'."*

## Standards (Baked In)

Apply these rules throughout. They mirror the `research` phase of `standards/standards-manifest.yml`.

- **[PRIN-1] User-First** — prioritize user value in every recommendation
- **[PRIN-2] Quality Over Speed** — do not cut corners on research depth
- **[PRIN-3] Transparency** — document limitations, unknowns, and assumptions
- **[SEC-1] No Secrets** — never include credentials or tokens in the RSD
- **[SEC-2] PII Protection** — do not include personal data in research docs
- **[CODE-ARCH]** — when surveying the codebase, note existing patterns, integration points, and reusable abstractions rather than prescribing a new architecture
- **[CONTENT-VOICE]** — for content tracks, capture existing brand/voice signals before suggesting new ones
- **[DESIGN-UI]** — for design tracks, prefer existing tokens/components; call out where the design system is silent

Record a **Standards Compliance** section at the end of the RSD noting the manifest version (if available), which of the above were applied, and any deviations with rationale.

## Process

### 1. Ask Clarifying Questions (Required)

Before any research, ask **4–7 essential clarifying questions**. Format:
- **Numbered** (1, 2, 3, …)
- **Multiple choice** with options labeled **A, B, C, D, …**
- Phrased so the user can respond with selections like `1A, 2C, 3B, 4D`

Always cover:
- Project type(s): Product / Content / Design (or combinations)
- Research depth: quick scan / moderate / deep dive
- Internal vs. external focus
- Current state: net-new / extending / migrating
- Key constraints (performance, security, a11y, brand, time-to-market…)
- Internal resources to emphasize
- External research priorities

### 2. Wait for Responses

**PAUSE.** Do not begin research until the user answers.

### 3. Plan Scope

Decide which tracks apply, depth, internal/external balance, and the primary outputs needed for the upcoming PRD/CRD/DRD.

### 4. Internal Research

Explore:
- Existing `./assets/rsd-*.md`, `prd-*.md`, `crd-*.md`, `drd-*.md`, `tasks-*.md`
- Codebase: services, modules, components, integration points, constraints, reusable utilities
- Content assets and brand/voice guidelines (content tracks)
- Design system tokens and components (design tracks)

### 5. External Research

When appropriate, use web search for:
- Domain / framework best practices
- Reference implementations
- Platform guidelines (HIG, Material, WCAG, etc.)
- Standards and compliance (WCAG 2.x, GDPR, HIPAA, …)

Summarize in your own words. Prefer official documentation and reputable, up-to-date sources.

### 6. Generate the RSD

Use this structure. Omit sections that clearly do not apply. Scale detail to the chosen depth.

```markdown
# Research Summary Document (RSD): [Project Name] v[version]

## 1. Project Overview
- User brief, project type(s), research depth, primary focus

## 2. Existing Context & Assets (Internal)
### 2.1 Related Requirements & Docs
### 2.2 Codebase / System Context (Product/Design)
### 2.3 Content & Brand Context (Content)
### 2.4 Design System Context (Design)

## 3. User & Business Context
- Target users, goals, pain points, success signals

## 4. External Research: Best Practices & References
### 4.1 Domain & Framework Best Practices
### 4.2 Reference Implementations / Examples
### 4.3 Standards, Compliance, and Accessibility

## 5. Constraints, Risks, and Dependencies
### 5.1 Constraints (technical / organizational / brand-legal)
### 5.2 Risks (impact, likelihood, mitigation)
### 5.3 Dependencies & Assumptions

## 6. Opportunities & Ideas
- Reuse opportunities, quick wins, differentiation, future extensions

## 7. Key Findings by Track
### 7.1 Product / Feature Findings (if applicable)
### 7.2 Content Findings (if applicable)
### 7.3 Design Findings (if applicable)

## 8. Recommendations for the Create Phase
### 8.1 Recommended requirements doc(s) and filenames
### 8.2 Scope recommendations (MVP vs. stretch)
### 8.3 Key questions the requirements doc should answer
### 8.4 Suggested decisions to lock in now

## 9. Open Questions & Gaps

## 10. Sources & References

## 11. Standards Compliance
- Standards applied, manifest version (if available), deviations
```

### 7. Save the RSD

- **Location:** `./assets/`
- **Filename:** `rsd-[project-name]-v[version].md`
- Create `./assets/` if missing.
- **NEVER** re-write, revise, or overwrite a file prefaced `rsd-`. Always bump the version (`-v1` → `-v2`).

## Target Audience

The RSD is read by (a) the user making requirements decisions and (b) the AI that will write the next PRD/CRD/DRD. Findings must be explicit, actionable, and tied to decisions the Create phase must make.

## What NOT to Do

- Do NOT write requirements in the RSD — research, not specification
- Do NOT skip clarifying questions
- Do NOT overwrite existing `rsd-*` files — bump the version
- Do NOT begin the Create phase — that is a separate skill
