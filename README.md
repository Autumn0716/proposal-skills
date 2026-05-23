<div align="center">

# Proposal

**缩小你和 Agent 之间的信息差**

</div>

<div align="center">

*Born from the philosophy of* [superpowers:brainstorming](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/superpowers) *and* [grill-me](https://github.com/anthropics/claude-plugins-official)*— two skills that taught us: the hardest part of building something isn't writing code, it's making sure you're building the right thing.*

</div>

---

## 为什么需要 Proposal

你一定经历过这种时刻——

你对 Agent 说"加个功能"，它秒速开始写代码。十分钟后交付了，你一看：方向偏了，细节错了，该想的没想。你叹口气说"不是这个意思"，它改了一版又偏到另一边。来来回回，沟通成本远大于编码成本。

**这不是 Agent 不够聪明。这是你和它之间横亘着一道信息差。**

就像跨部门协作——你脑中装着完整上下文：为什么要做、给谁做、做了之后会怎样。而对方只拿到一句话。你以为自己说清楚了，对方以为自己理解了，双方各怀假设往前冲，交付日才发现——做出来的不是你要的。

这个问题的根源不是智力，是**对齐**。信息从你的脑中传递到 Agent 的上下文，每经过一个环节都在损耗。你说的和它理解的，从来不是同一件事。

`superpowers:brainstorming` 教会我们：在动手之前，先把想法变成设计，把设计变成文档，把文档交给用户过目。一行代码都不写，直到用户点头说"这就是我要的"。

`grill-me` 教会我们：追问要狠，一个问题一个问题地剥，每个决策分支都走到底，能从代码库找到答案的就不问用户。不把模糊地带留到实现阶段。

`proposal` 把这两种哲学熔铸成一道不可跳过的工序——**四道闸门，每次交付前强制校准。信息差在每道关口被逐步压缩，直到归零。**

> **All you need is `/proposal`.**

---

## 四道闸门

```
 Elicit ────▶ Plan ────▶ Execute ────▶ Review
   │            │           │            │
   🎯           📋          ⚡           ✅
```

| 闸门 | 做什么 | 对齐承诺 |
|------|--------|----------|
| 🎯 **Elicit** | brainstorm 追问 → grill-me 深挖 | 设计文档经你批准前，一行代码都不写 |
| 📋 **Plan** | 制定计划书 → 独立审计 | 第三方审查覆盖率 + 方案质量，不过则重来 |
| ⚡ **Execute** | todo 驱动，逐步验证 | 禁绝 scope creep，三步不过即回退 |
| ✅ **Review** | 改动 vs 需求双向追溯 | 任何一个 ❌ 都保持 goal 活跃，绝不交付半成品 |

四道闸门，环环相扣。**Elicit** 确保"做正确的事"，**Plan** 确保"正确地做事"，**Execute** 确保"按计划做事"，**Review** 确保"做到了再说做到了"。一个目的：**交付那一刻，不再有意外。**

---

## 安装

```bash
mkdir -p ~/.claude/skills/proposal/references && \
curl -fsSL https://raw.githubusercontent.com/Autumn0716/proposal-skills/main/SKILL.md -o ~/.claude/skills/proposal/SKILL.md && \
curl -fsSL https://raw.githubusercontent.com/Autumn0716/proposal-skills/main/references/grill-questions.md -o ~/.claude/skills/proposal/references/grill-questions.md && \
curl -fsSL https://raw.githubusercontent.com/Autumn0716/proposal-skills/main/references/plan-template.md -o ~/.claude/skills/proposal/references/plan-template.md && \
curl -fsSL https://raw.githubusercontent.com/Autumn0716/proposal-skills/main/references/traceability-matrix.md -o ~/.claude/skills/proposal/references/traceability-matrix.md
```

安装后即可使用：
- 手动调用：`/proposal`
- 自动触发：说 "add a feature"、"implement X"、"make a proposal"

---


# Proposal — Structured Feature Development

Never jump to implementation. Complete each phase gate before proceeding.

## Phase 1: Elicit — Understand

Invoke `/superpowers:brainstorming` first. Do not write any code until the design is approved. Then use `grill-me` to resolve remaining ambiguities the design doc left open.

**Gate**: User confirms "that's everything" on a written requirements summary.

## Phase 2: Plan — Create and Audit

Write an implementation plan to `docs/plan/<feature>-<YYYY-MM-DD>.md` using the template in `references/plan-template.md`.

Then spawn an independent reviewer agent to audit the plan against Phase 1 requirements. The audit must assess both coverage (every req has a step, every step has a req) and quality (is the approach viable, are edge cases handled, are dependencies correct). If FAIL, revise and re-audit until PASS.

**Gate**: Plan passes audit AND user approves.

## Phase 3: Execute — Goal-Tracked Implementation

Create a todo list from the plan steps. Implement one step at a time, verifying each (lint, type-check, test) before marking complete. If a step fails verification 3 times, revert and consult a senior reviewer agent.

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
