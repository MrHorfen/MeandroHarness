# MeandroHarness

一套面向 AI Agent 的工程治理体系（Harness），用于约束多智能体的分工、协作与验收。本仓库是 **脱敏后的公开发布版**，供参考与复用。

## 这是什么

Harness 是一组约束 AI 代理如何思考、做事与验收的工程纪律，包含四层治理：

| 层级 | 目录 | 作用 |
|------|------|------|
| 顶层宪法 | [`rules/agent_constitution.md`](rules/agent_constitution.md) | 怎么想：核心原则、真源优先级、推理闸、错误分级、验收规则 |
| Agent 定义 | [`agents/`](agents/) | 谁做什么：12 个职责角色（调度中枢、质量校验、安全审计等），每个一个 AGENT.md |
| 业务规则 | [`rules/project_rules.md`](rules/project_rules.md) | 怎么做：代码注释规范、文件操作安全、沙箱隔离等通用工序 |
| 决策记录 | [`notes/`](notes/) | 为什么这么做：架构决策、设计取舍、审计记录，四态流转（proposed→implemented→rejected/archived） |

## 目录结构

```
.
├── agents/    # 12 个 Agent 定义（master-controller、quality-inspector 等）
├── skills/    # 18 个通用技能（git-commit、frontend-skill、data-analysis 等）
├── rules/     # 工程宪法 + 项目规则 + 记忆晋升规则
├── notes/     # 决策记录体系
└── logs/      # 技能总览（SKILLS_OVERVIEW.md）
```

## 如何接入

在任意 AI 工作区（如 Trae）的根目录创建 `.trae/` 文件夹，将本仓库 `agents/`、`skills/`、`rules/`、`notes/` 内容放入其中，AI 辅助开发工具即可按这套治理体系协作。

## 发布说明（脱敏声明）

本公开版已做以下处理：

- **已排除**：`ima-skill`（含第三方凭据接入逻辑）、内部敏感文档、历史审计快照
- **已替换**：本机绝对路径 → 相对路径占位；内部组件引用 → 通用表述
- **保留**：通用治理规则、决策记录（含直接 ima-skill 的说明均已消除）

> 如需完整工作版（含 ima-skill 等业务组件），请使用内部仓库，勿从本公开库获取。

## 授权

AGENT.md 与 SKILL.md 均以类似 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 的精神开放参考，使用时请注明来源。