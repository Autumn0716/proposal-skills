# Elicit Phase — Skill Integration Guide

This reference maps the two Elicit-phase skills (`/superpowers:brainstorming` and `grill-me`) to the proposal workflow, ensuring their actual mechanisms are respected rather than bypassed.

---

## Step 1: Brainstorming — From Idea to Design

**When to use**: User describes a new idea, feature, or change. The goal is to turn a raw idea into a validated design spec.

**Skill invoked**: `/superpowers:brainstorming`

### HARD-GATE (non-negotiable)

> Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until a design has been presented and the user has approved it. This applies to EVERY project regardless of perceived simplicity.

### 9-Step Checklist (complete in order)

1. **Explore project context** — check files, docs, recent commits
2. **Offer visual companion** (if visual questions ahead) — its own message, nothing else in that message
3. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
4. **Propose 2-3 approaches** — with trade-offs and your recommendation
5. **Present design** — in sections scaled to complexity, get approval after each section
6. **Write design doc** — save to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` and commit
7. **Spec self-review** — check for placeholders, contradictions, ambiguity, scope; fix inline
8. **User reviews written spec** — ask user to review the spec file before proceeding
9. **Transition to implementation** — invoke `writing-plans` skill

### Key Principles to Enforce

- **One question at a time** — never batch
- **Multiple choice preferred** — easier to answer than open-ended when possible
- **YAGNI ruthlessly** — remove unnecessary features from all designs
- **Explore alternatives** — always propose 2-3 approaches before settling
- **Incremental validation** — present design section by section, get approval before moving on
- **Assess scope first** — if request describes multiple independent subsystems, flag and decompose before refining details
- **Existing codebase** — explore current structure first, follow existing patterns, include targeted improvements only if they serve the current goal

### Output

A committed spec document at `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`.

### Terminal State

After brainstorming, the ONLY next step is `writing-plans`. Do NOT invoke any other implementation skill.

---

## Step 2: Grill-Me — Deep Requirement Extraction

**When to use**: When the user has a direction but requirements are still fuzzy or incomplete. Also use when the brainstorming phase surfaces ambiguities that need resolution.

**Skill invoked**: `grill-me`

### Core Mechanism (MUST follow)

grill-me has four hard rules that override any generic question template:

1. **Ask one question at a time** — never batch questions. Wait for the answer before asking the next.
2. **Resolve each decision branch before moving on** — walk down the design tree, resolving dependencies between decisions one-by-one. Do not leave branches open.
3. **Explore the codebase first** — if a question can be answered by reading the codebase, explore the codebase instead of asking the user. The user should only answer questions the code cannot answer.
4. **Provide a recommended answer** — for each question, offer your informed suggestion. Let the user confirm or override.

### How grill-me Complements Brainstorming

- **Brainstorming** answers "what are we building and why?" — produces the design spec
- **Grill-me** answers "what are the exact requirements and edge cases?" — resolves every branch of the decision tree
- Use brainstorming FIRST for ideation, then grill-me to sharpen and validate
- If brainstorming already resolved all ambiguities, grill-me may be short or skipped

---

## Step 3: Elicit Gate — Summary and Confirmation

After both skills complete, produce the requirement summary:

```markdown
## Requirements Summary

### Problem
<from brainstorming — the validated design intent>

### Scope
**IN**: <from brainstorming spec + grill-me clarifications>
**OUT**: <from brainstorming spec + grill-me clarifications>

### Success Criteria
<testable criteria — must be specific enough to verify>

### Constraints
<from codebase exploration + user answers>

### Open Questions
<any unresolved items — must be zero before proceeding>
```

Present to user. Wait for explicit "that's everything" or "nothing more to add" before proceeding to Phase 2.

---

## Anti-Patterns

- ❌ Writing code before design is approved (violates brainstorming HARD-GATE)
- ❌ Skipping brainstorming and going straight to grill-me (direction not established)
- ❌ Asking multiple questions in one message (violates both skills' "one at a time" rule)
- ❌ Asking the user things that could be found by exploring the codebase (violates grill-me rule 3)
- ❌ Not providing a recommended answer for each grill-me question (violates grill-me rule 4)
- ❌ Accepting "this is too simple to need a design" (brainstorming explicitly rejects this)
- ❌ Skipping the 2-3 approaches step (violates brainstorming "explore alternatives" principle)
- ❌ Jumping from brainstorming to implementation without writing-plans (violates brainstorming terminal state)
