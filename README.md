# Proposal

> **All you need is `/proposal`.**

你和 Agent 之间，隔着一道信息差。

就像工作中把需求交给同级部门——你脑子里有完整的上下文，对方只拿到一句话。你以为说清楚了，对方以为理解了。两边都按自己的假设往前冲，交付那天才发现：做出来的东西不是你要的。

这不是 Agent 不够聪明。是你和它之间少了一道**强制对齐**的工序。

`proposal` 把这道工序做成不可跳过的流程：

| 阶段 | 做什么 | 怎么保证对齐 |
|------|--------|------------|
| **Elicit** | brainstorm → grill-me 追问到底 | 设计文档必须经你过目批准，一行代码都不写 |
| **Plan** | 制定计划书 → subagent 审计 | oracle/momus 审查覆盖率 + 方案质量，不过则重来 |
| **Execute** | todo 驱动逐步实现 | 禁绝 scope creep，三步验证，失败即回退 |
| **Review** | 改动 vs 需求双向追溯 | 任何一个 ❌ 都保持 goal 活跃，不交付半成品 |

四道闸门。一个目的：**让你和 Agent 在每道关口校准一次，交付那一刻不再有意外。**

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
