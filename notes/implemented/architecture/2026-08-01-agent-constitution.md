# Agent 宪法 — 错误分级 + 推理闸 + 真源优先级

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：用户 + master-controller
- **关联**：`.trae/rules/agent_constitution.md`、`.trae/rules/project_rules.md`

## 背景

HARNESS 初始状态只有 `project_rules.md`（业务规则），缺少顶层工程纪律。Agent 之间引用混乱、规则冲突无裁决标准、编码前无强制检查点。需要一份"宪法级"文件，优先级高于所有 Agent 定义和业务规则。

## 决策

创建三层治理体系：

1. **agent_constitution.md**（顶层）— 约束所有 Agent 的"怎么想、怎么做、怎么验收"
2. **Agent 定义文件**（中层）— 职责、协作、约束
3. **project_rules.md**（业务层）— 业务逻辑规则

核心机制：
- **错误分级体系**：🔴阻断 / 🟠设计风险 / 🟡可记录债务 / 🟢无关优化
- **推理闸**：编码前必答 7 问，不闭合不开始
- **真源优先级**：源码 > 规则 > 计划 > 文档 > 记忆
- **停止清单**：8 种必须停止询问用户的情况

## 备选方案

1. **只扩展 project_rules.md**：不选。业务规则和工程纪律混在一起，无法区分优先级
2. **每个 Agent 独立定义纪律**：不选。会导致规则分散、标准不一
3. **用 AGENTS.md 作为唯一入口**：不选。DeepSeek 的做法适用于单一仓库，我们有多项目复用需求

## 影响

- 新增 `agent_constitution.md`（9780 bytes）
- 所有 12 个 Agent 必须引用宪法
- `project_rules.md` 降级为业务规则层
- quality-inspector 校验时以宪法为准绳