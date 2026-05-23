---
name: proposal
description: This skill should be used when the user asks to "add a feature", "implement something", "create a new functionality", "build X", "make a proposal", "I want to add", "I need a new", or describes a project-related change that requires planning before implementation. Provides a structured 4-phase workflow: Elicit → Plan → Execute → Review, ensuring complete requirement understanding, plan-user alignment, and change verification before delivery.
disable-model-invocation: false
---

# Proposal - Structured Feature Development Workflow

Execute a rigorous 4-phase workflow for any non-trivial project change. Never jump to implementation without completing each phase gate.

## Overview

Most implementation failures stem from misunderstood requirements, misaligned plans, or unverified changes. This workflow enforces discipline at each handoff point:

1. **Elicit** — Understand intent completely before writing a single line
2. **Plan** — Create a plan, then audit it against the user's actual words
3. **Execute** — Implement with active goal tracking
4. **Review** — Verify every change maps back to the plan and the user's request

Proceed to the next phase only when the current phase passes its gate. If a gate fails, loop back.

---

## Phase 1: Elicit — Understand Before Acting

**Gate**: User confirms design is complete and approves the written spec.

### Step 1.1: Brainstorm (Turn Idea into Validated Design)

Invoke `/superpowers:brainstorming` immediately. Do NOT attempt to answer or implement directly.

**HARD-GATE**: No implementation code, no scaffolding, no implementation skill invocation until a design has been presented and approved. This applies to EVERY project regardless of perceived simplicity.

**Enforce the 9-step checklist**, in order:
1. Explore project context — check files, docs, recent commits
2. Offer visual companion (if visual questions ahead) — as its own message
3. Ask clarifying questions — one at a time, understand purpose/constraints/success criteria
4. Propose 2-3 approaches — with trade-offs and your recommendation
5. Present design — in sections scaled to complexity, get approval after each section
6. Write design doc — save to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` and commit
7. Spec self-review — check for placeholders, contradictions, ambiguity, scope; fix inline
8. User reviews written spec — wait for explicit approval before proceeding
9. Transition to implementation — invoke `writing-plans` skill

**Key principles to enforce**: One question at a time, multiple choice preferred, YAGNI ruthlessly, explore alternatives, incremental validation. If the request describes multiple independent subsystems, flag and decompose first.

**Terminal state**: After brainstorming, the ONLY next skill is `writing-plans`. Do NOT invoke any other implementation skill.

### Step 1.2: Deep Grilling (Resolve Every Decision Branch)

After the brainstorming design spec exists, use `grill-me` to sharpen ambiguous requirements.

**Enforce grill-me's 4 hard rules**:
1. **Ask one question at a time** — wait for the answer before asking the next
2. **Resolve each decision branch before moving on** — walk down the design tree, do not leave branches open
3. **Explore the codebase first** — if a question can be answered by reading the codebase, explore instead of asking the user
4. **Provide a recommended answer** — for each question, offer your informed suggestion and let the user confirm or override

Cover what brainstorming left unresolved:
- Exact scope boundaries (what is explicitly IN and OUT)
- Edge cases and error handling
- Non-obvious dependencies and constraints
- Verifiable success criteria

### Step 1.3: Elicit Gate — Summarize and Confirm

Produce a concise requirement summary:
- Problem statement (from brainstorming design doc)
- Scope boundaries (from brainstorming spec + grill-me clarifications)
- Success criteria (testable, from grill-me)
- Known constraints (from codebase exploration + user answers)
- Open questions (must be zero before proceeding)

Present this to the user. Wait for explicit "that's everything" or equivalent confirmation before proceeding to Phase 2.

### Reference

For detailed skill integration mapping, consult **`references/grill-questions.md`**.

---

## Phase 2: Plan — Create and Audit

**Gate**: Plan passes alignment audit against Phase 1 requirements.

### Step 2.1: Create Plan Document

Write a detailed implementation plan to `docs/plans/<feature-name>-<timestamp>.md`. Include:

```markdown
# Proposal: <Feature Name>

## Requirements (from Elicit phase)
- <requirement 1>
- <requirement 2>

## Implementation Plan
### Step 1: <description>
- Files to modify: <paths>
- Approach: <how>
- Verify: <how to confirm>

### Step 2: <description>
...

## Success Criteria
- [ ] <criterion 1>
- [ ] <criterion 2>

## Out of Scope
- <explicitly excluded items>
```

### Step 2.2: Alignment Audit (MANDATORY)

Spawn a subagent (oracle or momus) to audit the plan against the user's original requirements. The audit must assess both **coverage** and **quality**:

**Audit prompt**:
```
TASK: Audit this plan against the user's requirements.

Read the plan at <path> and the requirements from Phase 1.

## Coverage check
- Does every plan step map to a stated requirement?
- Are there requirements with no corresponding plan step?
- Are there plan steps that go beyond the stated scope?

## Quality check (MANDATORY)
- Is the approach viable? Are there obvious architectural flaws?
- Are edge cases and error states handled?
- Are dependencies correct (no circular dependencies, missing deps)?
- Are success criteria testable and complete?
- Does the plan follow existing codebase patterns?
- Would a senior engineer approve this plan?

## Verdict
Return: PASS with confidence, or FAIL with specific gaps.
For FAIL: list every gap, its severity (blocker/major/minor), and a suggested fix.
```

If the audit FAILS:
1. Identify gaps from the audit
2. Revise the plan to address each gap
3. Re-run the audit (include the previous audit's findings in context)
4. Repeat until PASS

### Step 2.3: User Approval

Present the audited plan to the user. Wait for explicit approval before proceeding. If the user requests changes, update the plan and re-audit.

---

## Phase 3: Execute — Implement with Goal Tracking

**Gate**: All planned steps completed and verified.

### Step 3.1: Activate Goal

Set the goal using the plan document as the source of truth:

- Create detailed todo list from the plan steps
- Each todo must map to a specific plan step
- Mark the first todo `in_progress`

### Step 3.2: Implement Step by Step

For each plan step:

1. Mark todo `in_progress`
2. Implement the change
3. Verify locally (lint, type-check, test)
4. Mark todo `completed`
5. Move to the next step

If a step fails verification:

1. Fix the immediate issue (minimal fix only)
2. Re-verify
3. If 3 consecutive failures: STOP, revert, consult oracle

### Step 3.3: No Scope Creep

During execution, do NOT:

- Add features not in the plan
- Refactor adjacent code
- Fix pre-existing issues (unless blocking the current change)
- Change interfaces beyond what the plan specifies

If a new requirement emerges mid-execution, note it but do NOT implement it. Return to Phase 1 for the new requirement.

---

## Phase 4: Review — Verify Alignment

**Gate**: Every change maps to a plan step, and every plan step maps to a user requirement.

### Step 4.1: Change Audit

Compare all changes (git diff) against the plan:

- For each changed file/hunk, identify which plan step it belongs to
- Flag any changes NOT covered by the plan
- Flag any plan steps with NO corresponding changes

### Step 4.2: Requirement Traceability

Build a traceability matrix:

| User Requirement | Plan Step | Files Changed  | Verified |
| ---------------- | --------- | -------------- | -------- |
| <req 1>          | <step 2>  | <file1, file2> | ✅ / ❌    |
| <req 2>          | <step 3>  | <file3>        | ✅ / ❌    |

Every requirement must trace to at least one verified change. Every change must trace to at least one requirement.

### Step 4.3: Final Verification

1. Run project build/test suite — must pass
2. Run `lsp_diagnostics` on all changed files — must be clean
3. Verify each success criterion from the plan is met

If any check fails:

1. Do NOT deliver the final answer
2. Keep the goal active
3. Fix the issue
4. Re-run the review phase

### Step 4.4: Delivery

Only when ALL gates pass:

1. Present a summary of what was done
2. Reference the plan document
3. Note any deviations (there should be none)
4. Cancel background tasks
5. Mark the goal complete

---

## Quick Reference: Phase Gates

| Phase   | Gate                                      | Signal to Proceed             |
| ------- | ----------------------------------------- | ----------------------------- |
| Elicit  | User confirms completeness                | "That's all" / "Nothing more" |
| Plan    | Alignment audit PASSES + user approves    | Explicit "go ahead"           |
| Execute | All plan steps done + verified            | All todos `completed`         |
| Review  | Traceability matrix complete + tests pass | All checks green              |

## Anti-Patterns (NEVER)

- Skip Elicit to "save time" — requirements will be wrong
- Skip the alignment audit — the plan will drift from requirements
- Implement during Elicit — understanding is incomplete
- Add unplanned features during Execute — scope creep
- Deliver without Review — unverified changes are broken changes
- Close the goal if Review fails — fix first, deliver second

## Additional Resources

### Reference Files

- **`references/grill-questions.md`** — Full Elicit-phase skill integration guide: brainstorming 9-step checklist, grill-me 4 hard rules, gate procedures, and anti-patterns
- **`references/plan-template.md`** — Full plan document template with all sections
- **`references/traceability-matrix.md`** — Detailed traceability matrix format and examples
