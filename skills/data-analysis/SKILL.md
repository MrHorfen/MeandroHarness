---
name: "data-analysis"
description: "基于 Python/SQL 对 Excel/CSV/JSON 数据进行查询、聚合、统计与多表关联分析，含可视化图表生成（已吸收 chart-visualization）。触发：用户要「分析数据」、「统计一下」、「画个图」、「数据可视化」、「趋势图」、「柱状图」、「饼图」。不触发：数据清洗全流程→fullstack-code-developer；写分析报告→doc-coauthoring；设计海报/封面→canvas-design。"
---
# 数据分析与可视化技能

基于 Python/SQL 对 Excel/CSV/JSON 数据进行查询、聚合、统计与可视化，输出结构化结果和图表。

> **v2.0 合并说明**：已吸收 chart-visualization。数据分析 + 图表生成现在是一个 Skill，不再需要串联调用。

## 何时触发

✅ **WHEN TO USE:**
- 用户要「分析数据」、「统计一下」、「汇总数据」
- 用户要「画个图」、「生成图表」、「数据可视化」
- 用户要「趋势图」、「柱状图」、「饼图」、「折线图」
- 用户需要 SQL 查询、GROUP BY 聚合统计
- 用户需要多表 JOIN 关联分析、数据关联
- 用户需要窗口函数、子查询等高级统计
- 用户要做数据透视、分组汇总、同比环比
- 乐谱转码质量统计、识别准确率分析
- HyperFrames 项目数据统计（时长、场景数等）
- 报告/文档需要插入图表

❌ **WHEN NOT TO USE:**
- 数据清洗全流程（缺失值、异常值、格式转换）→ 让 fullstack-code-developer 写 Python 脚本
- 写分析报告、总结文档 → 用 doc-coauthoring
- 数据格式转换（JSON↔CSV↔Excel）→ 写脚本即可
- 网页数据采集 → 用 general-web-crawler
- 设计海报、封面、配图 → 用 canvas-design
- UI 界面图表 → 用 frontend-skill / frontend-design
- AI 生成图像 → 用 ai-image-generation

⚠️ **EDGE CASES:**
- 用户说「分析数据并出报告」→ 本 Skill 分析 + 画图，再 doc-coauthoring 写报告
- 用户说「分析数据并画图」→ 本 Skill 一站式搞定
- 用户说「帮我处理一下这个 Excel」→ 先确认：是统计分析（本 Skill）还是数据清洗/格式转换（写脚本）

## 支持的数据格式

| 格式 | 读取方式 |
|------|---------|
| Excel (.xlsx, .xls) | pandas read_excel |
| CSV | pandas read_csv |
| JSON | pandas read_json / json |
| 数据库 | sqlite3 / SQLAlchemy |

## 常见分析场景

### 基础统计
- COUNT / SUM / AVG / MIN / MAX
- 分组统计（GROUP BY）
- 排序、Top N、占比

### 关联分析
- 多表 JOIN（INNER / LEFT / RIGHT / FULL）
- UNION 合并
- 子查询、CTE

### 高级分析
- 窗口函数（ROW_NUMBER, RANK, 累计求和）
- 同比环比计算
- 透视表（pivot table）
- 交叉分析

### 乐谱转码专项
- 识别准确率统计
- 错误类型分布
- 多份乐谱质量对比
- 处理时长统计

### 图表可视化（原 chart-visualization）
- 根据数据特征自动选择合适的图表类型
- 趋势图（折线图、面积图）
- 对比图（柱状图、条形图、雷达图）
- 结构图（饼图、环形图、树图、桑基图）
- 分布图（直方图、箱线图、散点图）
- 支持 matplotlib / seaborn / plotly

## 结果输出

- 表格形式展示（Markdown 表格）
- 可视化图表（PNG/SVG/HTML）
- 导出为 Excel/CSV 文件
- 统计摘要（关键指标）

## 与其他 Skill 的配合

| 任务 | 串联方式 |
|------|---------|
| 数据清洗 → 统计分析 | fullstack-code-developer → data-analysis |
| 统计分析 → 分析报告 | data-analysis → doc-coauthoring |
| 网页采集 → 数据分析 | general-web-crawler → data-analysis |
