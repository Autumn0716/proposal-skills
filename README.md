> Inspired by [superpowers:brainstorming](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/superpowers) and [grill-me](https://github.com/anthropics/claude-plugins-official).

---


# Proposal — Structured Feature Development

Never jump to implementation. Complete each phase gate before proceeding.

## Phase 1: Elicit — Understand

Invoke `/superpowers:brainstorming` first. Do not write any code until the design is approved. Then use `grill-me` to resolve remaining ambiguities the design doc left open.

**Gate**: User confirms "that's everything" on a written requirements summary.

## Phase 2: Plan — Create and Audit

Write an implementation plan to `docs/plan/<feature>-<YYYY-MM-DD>.md` using the template in `references/plan-template.md`.

Then spawn oracle or momus to audit the plan against Phase 1 requirements. The audit must assess both coverage (every req has a step, every step has a req) and quality (is the approach viable, are edge cases handled, are dependencies correct). If FAIL, revise and re-audit until PASS.

**Gate**: Plan passes audit AND user approves.

## Phase 3: Execute — Goal-Tracked Implementation

Create a todo list from the plan steps. Implement one step at a time, verifying each (lint, type-check, test) before marking complete. If a step fails verification 3 times, revert and consult oracle.

**Gate**: All todos completed. No scope creep — do not add features, refactor adjacent code, or fix pre-existing issues not in the plan.

## Phase 4: Review — Verify Alignment

Compare all changes (git diff) against the plan. Build a traceability matrix using the format in `references/traceability-matrix.md`: every requirement → plan step → files changed → verified.

Run the project build and test suite. Run `lsp_diagnostics` on all changed files. Verify every success criterion from the plan is met.

If any check fails: do NOT deliver. Keep the goal active. Fix and re-run Review.

**Gate**: All checks green. Every change traces to a requirement, every requirement traces to a verified change.

## References

- `references/grill-questions.md` — Elicit phase: brainstorming 9-step checklist, grill-me 4 hard rules, anti-patterns
- `references/plan-template.md` — Full implementation plan template
- `references/traceability-matrix.md` — Review phase traceability matrix format and audit rules
