# Traceability Matrix — Review Phase Format

The traceability matrix is the core deliverable of Phase 4 (Review). It proves every change is justified and every requirement is met.

## Format

```
| # | User Requirement | Plan Step | Files Changed | Lines | Verified | Notes |
|---|-----------------|-----------|---------------|-------|----------|-------|
| 1 | <requirement>   | Step 2    | src/auth.ts   | 12-45 | ✅       |       |
| 2 | <requirement>   | Step 3    | src/api.ts    | 88-92 | ✅       |       |
```

## Column Definitions

- **#**: Sequential ID for cross-referencing
- **User Requirement**: The exact requirement from Elicit phase (quote the user's words)
- **Plan Step**: Which implementation step addresses this requirement
- **Files Changed**: Specific files modified for this requirement
- **Lines**: Approximate line ranges changed
- **Verified**: ✅ (test/build passes) or ❌ (failing)
- **Notes**: Deviations, caveats, or partial coverage

## Audit Rules

### Every Requirement → At Least One Change
If a requirement has no corresponding file change, the review FAILS. Either:
- The requirement was not implemented (go back to Execute)
- The requirement was implicitly met (document explicitly)

### Every Change → At Least One Requirement
If a file change maps to no requirement, it is unplanned work. Either:
- Remove the change (it's scope creep)
- Document why it was necessary (hidden requirement discovered during execution)

### No ❌ in Verified Column
If any row has ❌, the review FAILS. Fix the issue, re-verify, re-audit.

## Review Checklist

Before marking review complete:

- [ ] All requirements from Elicit phase appear in the matrix
- [ ] All changed files appear in at least one row
- [ ] No ❌ in the Verified column
- [ ] Build passes
- [ ] Test suite passes
- [ ] `lsp_diagnostics` clean on all changed files
- [ ] No unplanned changes in the diff
- [ ] Success criteria from the plan are all met

## Failure Protocol

If review fails:
1. Document which checks failed
2. Do NOT deliver the final answer
3. Keep the goal active
4. Fix the failing items
5. Re-run the entire review phase (not just the failed check)
