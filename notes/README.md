# Agent Notes — 决策记录体系

> 参照 DeepSeek Harness `.agents/notes/` 设计，2026-08-13 引入。
> 定位：记录 HARNESS 演进中的关键架构决策、设计取舍、规则矛盾裁决。
> 与 `agent_constitution.md`（怎么想）和 `project_rules.md`（怎么做）互补：Notes 记录"为什么这么做"。

---

## 状态流转

```
proposed ──→ implemented ──→ archived
                │
                └──→ rejected
```

| 状态 | 含义 | 目录 |
|------|------|------|
| **proposed** | 提案中，待讨论/审批 | `proposed/` |
| **implemented** | 已实施，当前有效 | `implemented/` |
| **rejected** | 被否决，记录原因 | `rejected/` |
| **archived** | 已过时，保留痕迹 | `archived/` |

---

## 何时写入

**必须写**（以下任一触发）：
- 架构取舍：owner 边界变更、职责拆分/合并、新模块引入
- 规则矛盾裁决：两个规则冲突时的最终裁决
- 争议性决定：用方案 A 而非 B，且 B 有明显支持者
- 宪法/规则文件新增或删除章节
- 外部依赖引入/移除（如 Remotion 移除）

**不必写**：
- 纯 bug 修复、拼写修正、格式调整
- 单文件小改动（< 20 行）
- 例行性更新（如版本号）

---

## 文件命名规范

```
YYYY-MM-DD-主题-kebab-case.md
```

示例：`2026-08-01-agent-constitution-adoption.md`

---

## 文件模板

```markdown
# 标题（一句话概括决策）

- **状态**：implemented
- **日期**：YYYY-MM-DD
- **决策者**：用户 / Agent 名称
- **关联**：相关文件路径

## 背景
（为什么需要做这个决策——问题、约束、触发条件）

## 决策
（做了什么决定——清晰、无歧义）

## 备选方案
（考虑过但没选的方案 + 不选的原因）

## 影响
（影响了哪些文件/Agent/流程/规则）
```

---

## 写入规则

1. **谁决策谁写**：做出决策的 Agent 或用户，在实施同一轮次写入
2. **一条一主题**：不合并多个不相关决策
3. **归档不编辑**：archived 目录下的记录冻结，不可修改
4. **关联必填**：标注受影响的文件路径，确保可追溯
5. **写入后更新 `project_memory.md`**：简要记录新增决策，避免遗漏

---

## 与现有体系的关系

| 现有 | 定位 | 与 Notes 的关系 |
|------|------|----------------|
| `agent_constitution.md` | 顶层宪法，约束"怎么想" | Notes 记录宪法条款的演进原因 |
| `project_rules.md` | 业务规则，约束"怎么做" | Notes 记录规则的取舍背景 |
| `project_memory.md` | 项目记忆，会话级 | Notes 是决策级，跨会话持久化 |
| `.trae/logs/` | 调度日志 | Notes 是设计决策，不是运行日志 |

---

## 当前决策索引

| 日期 | 主题 | 状态 | 位置 |
|------|------|------|------|
| 2026-08-01 | Agent 宪法（错误分级 + 推理闸 + 真源优先级） | implemented | `implemented/architecture/` |
| 2026-08-01 | 三轮审计：空壳 Agent 清理 + 引用修复 | implemented | `implemented/process/` |
| 2026-08-01 | 错误分级体系统一（🔴🟠🟡🟢） | implemented | `implemented/process/` |
| 2026-08-01 | GSAP 规则唯一 owner 归属 | implemented | `implemented/architecture/` |
| 2026-08-01 | quality-inspector 越级调度设计 | implemented | `implemented/architecture/` |
| 2026-08-01 | 孤岛/废弃 Skill 删除策略 | implemented | `implemented/process/` |
| 2026-08-01 | 宪法引用覆盖率 100% 要求 | implemented | `implemented/process/` |
| 2026-08-01 | 网络安全审查知识库整合 | implemented | `implemented/architecture/` |
| 2026-08-13 | 引入 DeepSeek Harness 决策记录体系 | implemented | `implemented/process/` |
| 2026-08-13 | 第四轮审计：残余无效引用清理 + 语义冲突修复 | implemented | `implemented/process/` |
| 2026-08-14 | 第五轮审计：Skill 总览重建 + 索引对齐 + conflicts_with 元数据净化 | implemented | `implemented/process/` |
| 2026-08-15 | 鼓点识别主方案替换：HorfenDrums → HorfenDrumsHybrid（混合注意力） | implemented | `implemented/process/` |
| 2026-08-15 | 鼓点风格渐进加深训练方案（Phase B 沙箱验证通过） | proposed | `proposed/process/` |
| 2026-08-18 | 会话知识晋升规则：否决 LOPD 式归档智能体，改规则驱动 | implemented | `implemented/process/` |
| 2026-08-19 | HorfenPiano v4 复兴训练方案（被 08-30 止损决策否决） | rejected | `rejected/process/` |
| 2026-08-30 | 钢琴路线止损：停止 HorfenPiano/HorfenScore 训练，生产维持 Basic Pitch，删 216GB 训练集 | implemented | `implemented/process/` |