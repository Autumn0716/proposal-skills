# proposal-skills

> 灵感源自 [`superpowers:brainstorming`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/superpowers) 和 [`grill-me`](https://github.com/anthropics/claude-plugins-official) 的设计哲学，将创意阶段的前置思考与严格的对齐审计和交付验证结合在一起。

## 概述

严肃的功能开发不应该从写代码开始。四个强制阶段，每阶段一个 Gate，不过不前进：

```
Elicit ──→ Plan ──→ Execute ──→ Review
  │          │          │           │
  Gate       Gate       Gate        Gate
```

| 阶段 | 说明 | 关键动作 |
|------|------|----------|
| **Elicit** | brainstorm + grill-me 追问直到完全理解意图 | `/superpowers:brainstorming` 9 步设计 → `grill-me` 深度需求提取 |
| **Plan** | 制定计划书 + subagent 严格对齐审计 | oracle/momus 对齐审计（覆盖率 + 质量双审计），不过则修订重审 |
| **Execute** | 按计划逐步实现，禁绝 scope creep | todo 跟踪，每步完成后验证，3 次失败即回退咨询 oracle |
| **Review** | 改动 vs 计划双向追溯，逐需求验证交付 | 可追溯性矩阵 + LSP 诊断 + 构建/测试全过，任何 ❌ 则 goal 保持活跃 |

## 快速开始

```bash
# 安装到 personal skills
cp -r skills/proposal ~/.claude/skills/proposal

# 或安装到 agents skills
cp -r skills/proposal ~/.agents/skills/proposal
```

触发方式：
- 手动调用：`/proposal`
- 自动触发：说 "add a feature"、"implement X"、"I want to add"、"make a proposal" 等

## 目录结构

```
proposal/
├── SKILL.md                          # 主文件：4 阶段完整工作流
└── references/
    ├── grill-questions.md            # Elicit 阶段 skill 整合指南
    ├── plan-template.md              # 计划书模板
    └── traceability-matrix.md        # 可追溯性矩阵格式
```
