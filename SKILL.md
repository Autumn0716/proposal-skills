---
name: proposal
description: This skill should be used when the user asks to "add a feature", "implement something", "create a new functionality", "build X", "make a proposal", "I want to add", "I need a new", or describes a project-related change that requires planning before implementation. Provides a structured 4-phase workflow: Elicit → Plan → Execute → Review, ensuring complete requirement understanding, plan-user alignment, and change verification before delivery.
disable-model-invocation: false
---

# Proposal — Structured Feature Development

Never jump to implementation. Complete each phase gate before proceeding.

## Phase 1: Elicit — Understand

Inject both `/superpowers:brainstorming` and `grill-me`. Use them iteratively — brainstorm to surface intent, grill-me to sharpen details, then loop back if new ambiguities emerge. Keep asking until both you and the user are fully satisfied that nothing is left unresolved. Do not write any code until the design is approved.

**Gate**: User confirms "that's everything" on a written requirements summary.

## Phase 2: Plan — Create and Audit

Write an implementation plan to `docs/plans/<feature>-<YYYY-MM-DD>.md` using the template in `references/plan-template.md`.

Then spawn an independent reviewer agent to audit the plan against Phase 1 requirements. The audit must assess both coverage (every req has a step, every step has a req) and quality (is the approach viable, are edge cases handled, are dependencies correct). If FAIL, revise and re-audit until PASS.

**Gate**: Plan passes audit AND user approves.

## Phase 3: Execute — Goal-Tracked Implementation

Create a task list from the plan steps using checkbox-style markers:

- `[ ]` — Pending (not started)
- `[✅]` — Completed (verified)
- `[⚠️]` — Warning (attempted but not actually fulfilled; needs attention)

Implement one step at a time, verifying each (lint, type-check, test) before marking `[✅]`. If a step was attempted but the verification did not actually pass, mark it `[⚠️]` — never pretend a task is done. If a step fails verification 3 times, revert and consult a senior reviewer agent.

**Gate**: All tasks marked `[✅]`. No `[⚠️]` remaining. No scope creep.

## Phase 4: Review — Verify Alignment

Compare all changes (git diff) against the plan. Build a traceability matrix using the format in `references/traceability-matrix.md`: every requirement → plan step → files changed → verified.

Run the project build and test suite. Run `lsp_diagnostics` on all changed files. Verify every success criterion from the plan is met.

If any check fails: do NOT deliver. Keep the goal active. Fix and re-run Review.

**Gate**: All checks green. Every change traces to a requirement, every requirement traces to a verified change.

## References

References live in `~/.agents/skills/proposal/references/`.

- `references/grill-questions.md` — Elicit phase: brainstorming 9-step checklist, grill-me 4 hard rules, anti-patterns
- `references/plan-template.md` — Full implementation plan template
- `references/traceability-matrix.md` — Review phase traceability matrix format and audit rules
