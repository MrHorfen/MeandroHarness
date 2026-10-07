---
name: "doc-coauthoring"
description: "通过分阶段协作流程组织与完善文档内容，生成结构清晰、逻辑完整的文稿。触发：用户要「写文档」、「写报告」、「整理内容」、「写README」、「生成分析报告」、「写技术方案」、「撰写文章」。不触发：写代码→用fullstack-code-developer；写视频剧本→用media-producer；海报/封面排版→用canvas-design。"
conflicts_with: ["canvas-design"]
---

# 文档协作技能

通过分阶段的协作流程组织与完善文档内容，生成结构清晰、逻辑完整的文稿。

## 何时触发

✅ **WHEN TO USE:**
- 用户要「写文档」、「写报告」、「整理内容」
- 用户要写 README、API 文档、使用指南
- 用户要生成分析报告、技术方案、总结文档
- 用户要撰写文章、整理知识、结构化输出
- 乐谱转码转换报告、质量评估报告
- HyperFrames 项目文档、制作说明
- 需求文档、设计文档、技术方案

❌ **WHEN NOT TO USE:**
- 写代码 → 用 fullstack-code-developer
- 写视频剧本/策划 → 用 media-producer
- 海报/封面/图文排版 → 用 canvas-design
- 数据分析 → 用 data-analysis
- 生成图表 → 用 data-analysis（已吸收 chart-visualization）
- 代码提交 → 用 git-commit

⚠️ **EDGE CASES:**
- 用户说「分析一下并出个报告」→ 先 data-analysis 分析，再 doc-coauthoring 写报告
- 用户说「写一份带图表的报告」→ data-analysis + doc-coauthoring 配合
- 用户说「写视频脚本」→ 用 media-producer，不是本技能

## 文档类型

### 技术文档
- README / 项目说明
- API 文档 / 接口文档
- 使用指南 / 操作手册
- 技术方案 / 架构设计文档
- 部署文档 / 运维手册

### 报告文档
- 分析报告 / 调研报告
- 质量评估报告 / 测试报告
- 转换报告 / 生成报告
- 项目总结 / 复盘报告

### 知识文档
- 教程 / 指南
- 最佳实践 / 经验总结
- 知识库整理
- 问题排查手册

## 协作流程（四阶段）

### 阶段一：大纲构建
- 明确文档目标和受众
- 规划内容结构
- 列出章节大纲
- 确认关键要点

### 阶段二：内容填充
- 逐章节展开
- 补充细节内容
- 添加示例说明
- 插入图表数据

### 阶段三：审核优化
- 检查逻辑连贯
- 优化表达方式
- 统一术语风格
- 完善格式排版

### 阶段四：定稿输出
- 最终审核确认
- 格式化输出
- 生成多格式版本

## 写作规范

- 结构清晰、层次分明
- 逻辑连贯、论证充分
- 语言简洁、表达准确
- 格式规范、排版美观
- 面向读者，避免自说自话

## 乐谱转码报告模板

```markdown
# 乐谱转换报告

## 基本信息
- 曲目标题：
- 作曲家：
- 乐器类型：
- 原始页数：
- 处理日期：

## 转换结果
- 输出格式：MusicXML
- 总小节数：
- Part 数量：
- 文件大小：

## 质量评估
- 调号识别：✅/❌
- 拍号识别：✅/❌
- 音符准确率：约 XX%
- 主要错误类型：

## 修正说明
- 已修正的问题：
- 未修正的问题（需人工核对）：

## 备注
```

## 与其他 Skill 的配合

| 任务 | 串联方式 |
|------|---------|
| 数据分析 + 报告 | data-analysis → doc-coauthoring |
| 图表 + 报告 | data-analysis → doc-coauthoring（图表已合并入 data-analysis） |
| 代码 + 文档更新 | fullstack-code-developer → doc-coauthoring |
| 头脑风暴 + 方案文档 | brainstorming → doc-coauthoring |
| 乐谱转码 + 报告 | pdf-to-musicxml-converter → doc-coauthoring |
