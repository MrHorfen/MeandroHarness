# 宪法引用覆盖率 100% 要求

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：master-controller
- **关联**：所有 12 个 Agent 的 AGENT.md

## 背景

审计发现 9 个 Agent（75%）未引用 `agent_constitution.md`，包括核心 Agent 如 `fullstack-code-developer`、`ai-training-master`、`cybersecurity-auditor`。未引用宪法的 Agent 可能忽略宪法条款（推理闸、错误分级、禁止补丁式开发等），导致执行偏离标准。

## 决策

**所有 Agent 必须显式引用 `agent_constitution.md`**，且每个 Agent 的引用语与其职责绑定：

| Agent | 引用强调的宪法条款 |
|-------|------------------|
| fullstack-code-developer | 推理闸 + 禁止补丁式开发 |
| gsap-animation-master | 渲染验证 + 禁止随机函数 |
| hyperframes-video-producer | GSAP 规则 owner 归属 |
| web-scraping-expert | 外部输入上界 |
| error-logging-optimizer | 不改变核心业务逻辑 |
| iteration-fallback | 先复现再修 |

## 备选方案

1. **只在 master-controller 中引用宪法**：不选。子 Agent 执行时可能不经过 master-controller
2. **不强制引用，靠文档约定**：不选。这正是导致 75% 缺失的根本原因

## 影响

- 所有 12 个 Agent 的 AGENT.md 增加宪法引用
- 覆盖率：25% → 100%
- 后续新增 Agent 必须包含宪法引用，quality-inspector 校验