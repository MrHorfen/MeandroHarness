# 会话知识晋升规则：否决 LOPD 式归档智能体，改规则驱动

- **状态**：implemented
- **日期**：2026-08-18
- **决策者**：用户 + 主 Agent
- **关联**：`.trae/rules/memory_promotion_rules.md`（新增）、`~/.trae-cn/memory/`（只读 + 晋升写入）、`.trae/notes/README.md`（索引）

## 背景

用户研读 LOPD（Latent On-Policy Self-Distillation）论文后提议：TRAE 多任务窗口需人工删除，简单删除无价值，是否可依据 LOPD 设"会话归档官"智能体，在删窗口前自蒸馏提取经验再删除，以提升 TRAE 效能。用户同时担心：与现有架构（文档维护工程师智能体、memory 体系）产生冗余，导致 RAG 检索质量损失。

## 决策

否决新建归档智能体，新增规则文件 `.trae/rules/memory_promotion_rules.md`：用户说"归档本次会话"时，当值 Agent 执行四分类提取（教训/决策/参数/风险）→ 查重 → 用户确认 → 晋升写入 project_memory.md（或 user_profile.md），重大架构决策进 `.trae/notes/`。

裁决依据：

1. **删窗口 ≠ 丢记忆**：TRAE 自动 memory 管线（session_memory.jsonl → topics.md）独立于任务窗口存在，删窗口不丢已持久化信息，"删除前蒸馏"的原始动机大部分已被默认满足
2. **LOPD 三前提在本场景不成立**：底层模型不可训练（无"内化进参数"的宿主）；旧会话无天然成败信号（无价值筛选依据）；收益错位（LOPD 省推理期上下文开销，删窗口省的是 UI 空间）
3. **真缺口只有一个**：topics.md → project_memory.md 晋升不稳定，且自动摘要是"发生了什么"流水账，非"学到什么"提炼——高价值结论（MuScriptor 推理链分析、hybrid 阈值教训等）滞留 topics 无人晋升

## 备选方案

1. **LOPD 式归档智能体（经验库 + 检索 + 压缩管线）**：不选。与 TRAE 自动 memory 管线大面积重叠；副本稀释 + 版本漂移会污染 RAG 索引（top-k 被副本挤占、命中过时版本）；会话文本经验库是无界增长的隐性运行面，违反宪法"外部输入上界"与"稳态优先"
2. **扩展文档维护工程师职责**：不选。其职责是代码 ↔ 用户文档同步（外部用户层），归档是会话 ↔ memory 晋升（内部真源层），合并违反分层解耦，且提示词臃肿
3. **不处理**：不选。晋升不闭环的缺口真实存在，且用户明确要求删窗口前先榨取价值

## 影响

- 新增 `.trae/rules/memory_promotion_rules.md`（workspace rule，常驻生效）
- 用户工作流新增口令："归档本次会话"
- 不新增智能体角色、不新增存储位置，RAG 索引面不变
- 后续 HARNESS 审计轮次增加检查项：project_memory.md 与 topics.md 的重复度/膨胀度
