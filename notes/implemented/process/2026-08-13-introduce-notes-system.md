# 引入 DeepSeek Harness 决策记录体系

- **状态**：implemented
- **日期**：2026-08-13
- **决策者**：用户 + master-controller
- **关联**：`.trae/notes/`（全量新增）、`agent_constitution.md`、`project_memory.md`

## 背景

对比 DeepSeek Harness 的 `.agents/notes/` 体系后，发现 HARNESS 缺少决策追溯机制。三轮审计中的重要决策（Agent 宪法引入、错误分级统一、GSAP owner 归属、Skill 清理策略等）仅存在于对话记忆中，无法跨会话检索。新成员或新 Agent 无法理解"为什么这么设计"。

## 决策

引入 Agent Notes 决策记录体系，参照 DeepSeek Harness 设计但适配 HyperFrames 的 `.trae/` 目录结构：

```
.trae/notes/
├── README.md              # 治理规则
├── implemented/
│   ├── architecture/      # 架构决策
│   └── process/           # 流程决策
├── proposed/              # 提案中
├── rejected/              # 被否决
└── archived/              # 已过时
```

四态流转：proposed → implemented → archived（或 → rejected）

## 备选方案

1. **用 project_memory.md 替代**：不选。project_memory.md 是会话级记忆，不持久化，且格式不统一
2. **用 Git commit message 替代**：不选。commit message 粒度太细，无法关联决策上下文
3. **完全照搬 DeepSeek 的 notes 结构**：不选。DeepSeek 的 notes 嵌入在 `.agents/` 下，与 agents 混合；我们独立为 `.trae/notes/` 更清晰

## 影响

- 新增 `.trae/notes/` 目录及 9 个文件（1 README + 8 决策记录）
- 回溯写入三轮审计中的 8 条关键决策
- `agent_constitution.md` 增加对 Notes 体系的引用
- 后续所有满足"必须写"条件的决策，必须在实施同一轮次写入