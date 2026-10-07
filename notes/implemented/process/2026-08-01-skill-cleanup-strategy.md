# 孤岛/废弃 Skill 删除策略

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：master-controller
- **关联**：`.trae/skills/`（全量）

## 背景

四类冗余 Skill 需要清理决策：

1. **孤岛 Skill**（无 Agent 引用）：`composition-patterns`（React 组件组合）、`react-best-practices`（React 代码审查）——本项目无 React 代码
2. **废弃 Skill**（有 deprecated 标记 + redirect_to）：`agent-browser`（→ general-web-crawler）、`chart-visualization`（→ data-analysis）——已合并到新 Skill

## 决策

**直接删除，不保留重定向**。

理由：
- 孤岛 Skill：无使用场景，保留只会增加维护负担
- 废弃 Skill：已确认无外部引用（所有 Agent 已更新引用），`redirect_to` 的价值有限，且平台系统提示词中仍列出旧 Skill 名，删除后让平台层同步清理

后续新增 Skill 时必须同步在 master-controller 调度表中注册，防止新的孤岛产生。

## 备选方案

1. **保留废弃 Skill 作为重定向**：不选。重定向文件本身也是维护负担，且平台层可能缓存旧 Skill 列表
2. **归档而非删除**：不选。无价值的 Skill 归档只是推迟删除

## 影响

- Skill 数量：23 → 19
- 删除的 Skill：composition-patterns、react-best-practices、agent-browser、chart-visualization
- 零孤岛、零废弃残留