# 第四轮审计：残余无效引用清理 + 语义冲突修复

- **状态**：implemented
- **日期**：2026-08-13
- **决策者**：master-controller + documentation-maintenance-engineer
- **关联**：`.trae/skills/`、`.trae/agents/`、`.trae/notes/README.md`

## 背景

前三轮审计已删除空壳 Agent 与孤岛 Skill，但清理后部分 **Skill 定义文件内仍残留对已删除/已合并组件的引用**，以及若干文档分层、分级语义的不一致。需第四次全面排查 Agent-Agent、Skill-Skill、Agent-Skill 交叉引用。

## 发现与修复

| # | 问题 | 类型 | 修复 |
|---|------|------|------|
| 1 | `doc-coauthoring`/`ai-image-generation`/`canvas-design` 引用已删除的 `chart-visualization` | Skill-Skill 无效引用 | 改为 `data-analysis`（已吸收 chart-visualization） |
| 2 | `frontend-skill` 引用已删除的 `composition-patterns`/`react-best-practices` | Skill-Skill 无效引用 | 删除残留引用 |
| 3 | `hyperframes` 引用已删除的 `remotion-video-engineer` | Skill-Agent 无效引用 | 删除残留引用 |
| 4 | `pdf-to-musicxml-converter` 引用不存在的 `AI-scoring-master`（项目无评分组件） | Skill-Skill 无效引用 | 删除 conflicts_with 与 body 引用 |
| 5 | `master-controller` 调度表将"小功能/片段"指派 fullstack，与 `fullstack-code-developer` 的"<50行直接执行"矛盾 | Agent-Agent 规则冲突 | 调度表补充"≥50行"边界，统一口径 |
| 6 | `cybersecurity-auditor` 的 🔴🟠🟡🟢 为漏洞严重度分级，与宪法错误分级符号相同、语义不同 | Agent-宪法 语义冲突 | 增加语义区分说明 |
| 7 | `documentation-maintenance-engineer` 文档分层表缺失"决策记录"层级（宪法§十一已扩展为四层） | Agent-宪法 不同步 | 补全决策记录层级 |
| 8 | `quality-inspector` Markdown 表格未闭合 `**` | 格式缺陷 | 修复 |
| 9 | `.trae/notes/README.md` 决策索引缺失 `error-rating-unification` 条目 | 索引不同步 | 补全 |

## 备选方案

保留引用并在旧 Skill 目录中放置 redirect 文件：不选。被引用的 Skill 已确认删除且无恢复计划，残留引用只会误导调度与文档检索。

## 影响

- 修改 `.trae/skills/` 下 6 个 Skill、`.trae/agents/` 下 3 个 Agent、`.trae/notes/README.md`
- `.trae/logs/` 与 `implemented/` 下历史决策记录**保持只读**，其内的旧组件名属于历史事实，不修改
- 本次为文档/调度规则维护，不涉及生产业务代码，无需沙箱验证
