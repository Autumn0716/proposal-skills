# Plan Document Template

Save plans to `.sisyphus/plans/<feature-name>-<timestamp>.md`.

```markdown
# Proposal: <Feature Name>

**Created**: <YYYY-MM-DD HH:MM>
**Status**: DRAFT | AUDITED | APPROVED | EXECUTING | COMPLETE

---

## Requirements (from Elicit Phase)

### Problem Statement
<1-2 sentences describing the core problem>

### Scope
**IN scope**:
- <item 1>
- <item 2>

**OUT of scope**:
- <item 1>
- <item 2>

### Success Criteria
- [ ] <criterion 1 — must be testable>
- [ ] <criterion 2 — must be testable>

### Constraints
- <constraint 1>
- <constraint 2>

### Open Questions
- [ ] <question 1>
- [ ] <question 2>

---

## Implementation Plan

### Step 1: <Short Description>

**Objective**: <What this step accomplishes>

**Files to modify**:
- `<path/to/file1>` — <what changes>
- `<path/to/file2>` — <what changes>

**Approach**: <How to implement — be specific>

**Verify**: <How to confirm this step works>

**Maps to requirement**: <req X>

---

### Step 2: <Short Description>

**Objective**: <What this step accomplishes>

**Files to modify**:
- `<path/to/file>` — <what changes>

**Approach**: <How to implement>

**Verify**: <How to confirm>

**Maps to requirement**: <req Y>

---

## Dependency Graph

```
Step 1 ──► Step 2 ──► Step 4
                   │
          Step 3 ──┘
```

Steps that can run in parallel: <list>
Steps that MUST be sequential: <list>

---

## Audit Result

**Auditor**: <oracle/momus>
**Result**: PASS / FAIL
**Confidence**: High / Medium / Low
**Gaps found**: <list or "none">
**Revisions**: <number of re-audits needed>

---

## Execution Log

| Step | Status | Started | Completed | Notes |
|------|--------|---------|-----------|-------|
| 1    | ⬜     |         |           |       |
| 2    | ⬜     |         |           |       |

---

## Review Traceability

| Requirement | Plan Step | Files Changed | Verified |
|-------------|-----------|---------------|----------|
| <req 1>     | <step 2>  | <file1, file2>| ✅ / ❌  |
| <req 2>     | <step 3>  | <file3>       | ✅ / ❌  |
```
