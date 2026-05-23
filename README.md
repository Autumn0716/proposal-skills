<div align="center">

# Proposal

**Narrow the information gap between you and your Agent**

</div>

<div align="center">

*Born from the philosophy of [superpowers:brainstorming](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md) and [grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)*

</div>

---

There's an information gap between you and your Agent — just like handing off work across departments. You have the full context in your head; they get a single sentence. You think you've explained it. They think they understand. By delivery day, what's built isn't what you asked for.

`brainstorming` taught us: write it down first, get user sign-off before a single line of code. `grill-me` taught us: dig deep, resolve every decision branch, leave no ambiguity for implementation. `proposal` fuses both into four non-negotiable gates — forced alignment before every delivery, compressing the information gap to zero.

> **All you need is `/proposal`.**

---

[中文版](#proposal-中文版)

---

## Four Gates

```
 Elicit ────▶ Plan ────▶ Execute ────▶ Review
   │            │           │            │
   🎯           📋          ⚡           ✅
```

| Gate | What it does | Alignment guarantee |
|------|-------------|---------------------|
| 🎯 **Elicit** | brainstorm → grill-me deep-dive | No code until you approve the design doc |
| 📋 **Plan** | Write plan → independent audit | Third-party reviews coverage + quality; redo if it fails |
| ⚡ **Execute** | Todo-driven, step-by-step verification | No scope creep; revert after 3 consecutive failures |
| ✅ **Review** | Changes vs. requirements bidirectional traceability | Any ❌ keeps the goal active; no half-done deliveries |

**Four gates. One purpose: no surprises at delivery.**

---

## Install

```bash
git clone https://github.com/Autumn0716/proposal-skills.git /tmp/proposal-skills

# Personal skills
cp -r /tmp/proposal-skills/SKILL.md /tmp/proposal-skills/references ~/.claude/skills/proposal/

# Or agent skills
cp -r /tmp/proposal-skills/SKILL.md /tmp/proposal-skills/references ~/.agents/skills/proposal/

# Cleanup
rm -rf /tmp/proposal-skills
```

- Manual: `/proposal`
- Auto-trigger: "add a feature", "implement X", "make a proposal"

---

---

<a id="proposal-中文版"></a>

<div align="center">

# Proposal

**缩小你和 Agent 之间的信息差**

</div>

<div align="center">

*诞生于 [superpowers:brainstorming](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md) 与 [grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md) 的设计哲学*

</div>

---

你和 Agent 之间，隔着一道信息差——就像跨部门交接，你脑子里有完整上下文，对方只拿到一句话。你以为说清了，对方以为懂了，交付才发现做的不是你要的。

`brainstorming` 教我们动手前先写文档、交用户过目；`grill-me` 教我们追问到底、模糊地带不留到实现。`proposal` 熔铸二者为四道不可跳过的闸门——交付前强制校准，信息差逐步压缩，直到归零。

> **All you need is `/proposal`.**

---

[English](#proposal)

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

**四道闸门。一个目的：交付那一刻，不再有意外。**

---

## 安装

```bash
git clone https://github.com/Autumn0716/proposal-skills.git /tmp/proposal-skills

# 安装到 personal skills
cp -r /tmp/proposal-skills/SKILL.md /tmp/proposal-skills/references ~/.claude/skills/proposal/

# 或安装到 agents skills
cp -r /tmp/proposal-skills/SKILL.md /tmp/proposal-skills/references ~/.agents/skills/proposal/

# 清理
rm -rf /tmp/proposal-skills
```

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
