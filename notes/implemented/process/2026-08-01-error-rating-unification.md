# 错误分级体系统一（🔴🟠🟡🟢）

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：master-controller
- **关联**：`agent_constitution.md §四`、`quality-inspector/AGENT.md`、`master-controller/AGENT.md`

## 背景

初始状态存在两套错误分级体系：
1. `agent_constitution.md` 定义：🔴阻断 / 🟠设计风险 / 🟡可记录债务 / 🟢无关优化
2. `quality-inspector/AGENT.md` 定义：格式偏差 / 内容偏差 / 逻辑偏差（旧表）
3. `master-controller/AGENT.md` 定义：🟢低 / 🟡中 / 🔴高（与宪法符号不一致）

三重体系导致执行时无法判断用哪个标准。

## 决策

统一使用宪法级分级体系，禁止任何 Agent 定义二级标准：

| 等级 | 符号 | 定义 | 处置 |
|------|------|------|------|
| 阻断 | 🔴 | 破坏核心功能、owner 边界、安全/隐私 | 当轮必须收掉 |
| 设计风险 | 🟠 | 会导致架构漂移、运行面失控 | 说明取舍和验收入口 |
| 可记录债务 | 🟡 | 不影响本轮目标链路 | 说明不处理原因 |
| 无关优化 | 🟢 | 不进入本轮范围 | 禁止借机扩大改造 |

## 备选方案

保留两套体系，按场景切换：不选。双重标准=没有标准。

## 影响

- 删除 `quality-inspector/AGENT.md` 旧偏差类型表
- 更新 `master-controller/AGENT.md` 风险处置策略表
- 所有 Agent 报告问题时统一使用 🔴🟠🟡🟢 符号