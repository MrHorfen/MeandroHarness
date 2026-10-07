# 三轮审计：空壳 Agent 清理 + 引用修复 + 冗余消除

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：master-controller
- **关联**：`.trae/agents/`（全量）、`.trae/skills/`（全量）

## 背景

HARNESS 初始状态混沌：
- 10 个 Agent 目录存在但无 AGENT.md（空壳），可能导致调度失败
- master-controller 调度表引用 4 个不存在的 Agent/Skill
- 4 个 Skill 被废弃但仍保留文件
- 2 个 Skill 未被任何 Agent 引用（孤岛）
- 3 个 Agent 引用已废弃 Skill（chart-visualization、agent-browser）
- 宪法引用覆盖率仅 25%（3/12）

## 决策

三轮逐步收敛：

**第一轮**：删除 10 个空壳 Agent 目录，清理 master-controller 调度表无效引用
**第二轮**：删除 4 个冗余 Skill（2 孤岛 + 2 废弃），统一 quality-inspector 分级体系
**第三轮**：修复 cybersecurity-auditor 的 `script-developer` 无效引用，宪法引用覆盖率达成 100%

清理原则：
- 空壳 Agent 目录：直接删除
- 孤岛 Skill：无使用场景则删除，有使用场景则在 master-controller 中补充引用
- 废弃 Skill：确认无外部引用后删除
- 无效引用：替换为正确的 Agent/Skill 名称

## 备选方案

保留所有空壳和废弃文件"以备后用"：不选。宪法禁止补丁式开发，空壳只会增加维护负担。

## 影响

- Agent 数量：22 → 12
- Skill 数量：27 → 19
- 交叉引用有效性：~60% → 100%
- 宪法引用覆盖率：25% → 100%
- 导出包大小：139 KB