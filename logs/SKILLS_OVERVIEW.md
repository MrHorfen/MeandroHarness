# Trae Skills 功能总览

本文档汇总了当前项目中已安装的所有 Trae 技能（Skill），按功能分类进行说明。

> **更新说明（2026-08-14）**：第五轮审计后重建。已删除 `react-best-practices`、`composition-patterns`、`chart-visualization`、`agent-browser` 四个历史条目（均已并入其他 Skill），并补齐 9 个现有 Skill。当前技能总数 **19 个**。

---

## 目录

- [开发工具类](#开发工具类)
- [AI 训练类](#ai-训练类)
- [前端开发类](#前端开发类)
- [测试自动化类](#测试自动化类)
- [数据处理类](#数据处理类)
- [视觉设计类](#视觉设计类)
- [文档协作类](#文档协作类)
- [浏览器自动化类](#浏览器自动化类)
- [网络安全类](#网络安全类)
- [动画特效类](#动画特效类)
- [视频制作类](#视频制作类)
- [音乐处理类](#音乐处理类)
- [知识库与笔记类](#知识库与笔记类)

---

## 开发工具类

### 1. git-commit

**功能定位**：代码提交辅助工具

**核心能力**：
- 根据 Conventional Commits 规范自动生成提交信息
- 分析代码变更，识别变更类型（feat、fix、docs、refactor 等）
- 按逻辑拆分与暂存文件
- 安全执行 Git 提交流程，不执行破坏性命令

**适用场景**：代码提交、生成规范的 commit message、分析 git diff

**配置文件**：[git-commit/SKILL.md](skills/git-commit/SKILL.md)

---

### 2. skill-search-master

**功能定位**：技能搜索与安装助手

**核心能力**：
- 分析任务需求，搜索社区技能库
- 筛选置信度 > 0.8 的高分技能
- 自动安装并调用匹配的技能

**适用场景**：当现有技能无法满足任务需求时自动触发

**配置文件**：[skill-search-master/SKILL.md](skills/skill-search-master/SKILL.md)

---

## AI 训练类

### 3. ai-training-knowledge

**功能定位**：AI 训练知识库

**核心能力**：
- 覆盖 DeepSeek/GLM/Qwen/LLaMA/Mistral 等主流大模型训练方案
- 超参配置、分布式策略、过拟合预防体系
- 持续学习方案、故障诊断方法

**适用场景**：ai-training-master 需要参考训练方案时自动加载

**配置文件**：[ai-training-knowledge/SKILL.md](skills/ai-training-knowledge/SKILL.md)

---

## 前端开发类

### 4. frontend-skill

**功能定位**：企业级功能型前端构建工具

**核心能力**：
- 信息层级设计（标题、正文、辅助信息）
- 排版规范（字体、行高、间距）
- 响应式设计（移动端优先、断点设置）
- 可访问性支持（键盘导航、屏幕阅读器）

**适用场景**：后台管理、数据面板、表单工具页

**冲突关系**：与 `frontend-design` 互斥（功能型 vs 创意型）

**配置文件**：[frontend-skill/SKILL.md](skills/frontend-skill/SKILL.md)

---

### 5. frontend-design

**功能定位**：高设计品质的创意型前端构建工具

**核心能力**：
- 视觉冲击力与创意表达
- 营销着陆页、品牌展示、作品集
- 前沿动效与沉浸式体验

**适用场景**：品牌官网、推广页、炫酷效果、营销页面

**冲突关系**：与 `frontend-skill` 互斥（创意型 vs 功能型）

**配置文件**：[frontend-design/SKILL.md](skills/frontend-design/SKILL.md)

---

## 测试自动化类

### 6. webapp-testing

**功能定位**：Web 应用自动化测试工具

**核心能力**：
- 基于 Playwright 生成并执行自动化测试脚本
- 支持多浏览器（Chrome、Firefox、Safari）
- 前端功能验证、界面调试与页面行为分析
- 测试执行与报告生成

**适用场景**：编写测试用例、自动化测试、页面功能验证

**配置文件**：[webapp-testing/SKILL.md](skills/webapp-testing/SKILL.md)

---

## 数据处理类

### 7. data-analysis

**功能定位**：数据查询与分析工具

**核心能力**：
- 基于 Python/SQL 对 Excel/CSV/JSON 数据查询、聚合、统计
- 多表关联分析
- 可视化图表生成（已吸收 chart-visualization）

**适用场景**：数据分析、统计汇总、数据可视化、趋势图/柱状图/饼图

**配置文件**：[data-analysis/SKILL.md](skills/data-analysis/SKILL.md)

---

## 视觉设计类

### 8. canvas-design

**功能定位**：静态视觉内容设计工具

**核心能力**：
- 海报设计（活动海报、宣传海报、产品海报）
- 封面设计（书籍、报告、演示文稿）
- 社交媒体配图（公众号封面、信息图）
- 通过 HTML/CSS/SVG 实现精确排版与图形设计

**适用场景**：设计海报、创建封面、配色构图类视觉设计

**冲突关系**：与 `ai-image-generation` 互斥（排版设计 vs 生成式图片）

**配置文件**：[canvas-design/SKILL.md](skills/canvas-design/SKILL.md)

---

### 9. ai-image-generation

**功能定位**：AI 生成式图片工具

**核心能力**：
- 通过 AI 模型（FLUX/Gemini/Grok/Seedream 等）生成图片
- 照片级写实图片、概念图、艺术图

**适用场景**：生成图片、AI 画图、写实照片、概念艺术

**冲突关系**：与 `canvas-design` 互斥（生成式图片 vs 排版设计）

**配置文件**：[ai-image-generation/SKILL.md](skills/ai-image-generation/SKILL.md)

---

## 文档协作类

### 10. doc-coauthoring

**功能定位**：文档协作编写工具

**核心能力**：
- 技术文档（API 文档、技术方案、系统设计）
- 产品文档（PRD、用户手册、发布说明）
- 报告文档（分析报告、总结报告、立项报告）
- 分阶段协作流程（大纲构建 → 内容填充 → 审核优化 → 定稿输出）

**适用场景**：编写文档、撰写文章、生成 README、整理内容

**冲突关系**：与 `canvas-design` 互斥（文稿 vs 排版）

**配置文件**：[doc-coauthoring/SKILL.md](skills/doc-coauthoring/SKILL.md)

---

### 11. brainstorming

**功能定位**：需求梳理与方案设计工具

**核心能力**：
- 需求澄清（目标、范围、约束、验收标准）
- 方案探索（多方案对比、复杂度评估）
- 交互式对话（逐步深入、确认决策）
- 输出生成（需求文档、设计方案）

**适用场景**：需求分析、方案设计、头脑风暴、需求模糊时

**冲突关系**：与 `doc-coauthoring` 互斥（方案设计 vs 文稿编写）

**配置文件**：[brainstorming/SKILL.md](skills/brainstorming/SKILL.md)

---

## 浏览器自动化类

### 12. general-web-crawler

**功能定位**：通用网页数据采集与爬虫专家

**核心能力**：
- 静态页面爬取 + 动态 JS 渲染
- 浏览器模拟操作（填表/点击/登录/截图）
- 反爬防护（User-Agent 轮换、请求延时）
- 数据导出（JSON/CSV/Markdown）

**适用场景**：爬取数据、采集信息、操作网页、填表单、登录后操作

> 已吸收 agent-browser（v2.0 合并）。

**配置文件**：[general-web-crawler/SKILL.md](skills/general-web-crawler/SKILL.md)

---

## 网络安全类

### 13. cybersecurity-audit-knowledge

**功能定位**：网络安全审计知识库

**核心能力**：
- OWASP Top 10 2026、CWE Top 25 2026
- OWASP LLM Top 10 2026（AI 应用安全）
- 等保数据安全新规 GA/T 2380-2026、2026 法规全景
- AI 项目安全审计检查清单

**适用场景**：cybersecurity-auditor 执行审计任务时自动加载

**配置文件**：[cybersecurity-audit-knowledge/SKILL.md](skills/cybersecurity-audit-knowledge/SKILL.md)

---

## 动画特效类

### 14. gsap

**功能定位**：GSAP 动画库参考与代码生成工具

**核心能力**：
- 时间线动画、缓动函数、入场出场效果
- 滚动触发（ScrollTrigger）
- 用于 HyperFrames 合成中的动画场景

**适用场景**：加动画、GSAP 动画、入场效果、转场动画

**冲突关系**：与 `hyperframes` 互斥（动画参考 vs 完整视频合成）

**配置文件**：[gsap/SKILL.md](skills/gsap/SKILL.md)

---

## 视频制作类

### 15. hyperframes

**功能定位**：HyperFrames HTML 视频合成项目创建与编辑

**核心能力**：
- 视频合成、动画、字幕、配音、音频可视化
- 场景转场、时间线编排

**适用场景**：做个视频、HyperFrames 项目、HTML 视频、动画视频、加字幕、做个片头

**冲突关系**：与 `hyperframes-cli`、`gsap` 互斥（内容创作 vs 渲染/动画参考）

**配置文件**：[hyperframes/SKILL.md](skills/hyperframes/SKILL.md)

---

### 16. hyperframes-cli

**功能定位**：HyperFrames CLI 开发循环工具

**核心能力**：
- 项目脚手架初始化（init）
- 代码校验（lint）、结构验证（inspect/validate）
- 预览（preview）、渲染 MP4（render）
- 环境排查（doctor）

**适用场景**：渲染视频、导出 MP4、hyperframes render、lint 检查、预览项目

**冲突关系**：与 `hyperframes` 互斥（渲染工具 vs 内容创作）

**配置文件**：[hyperframes-cli/SKILL.md](skills/hyperframes-cli/SKILL.md)

---

### 17. hyperframes-media

**功能定位**：HyperFrames 素材预处理工具

**核心能力**：
- 文本转语音（TTS 配音，edge-tts）
- 音频/视频转录（Whisper 字幕）
- 背景移除（透明化）

**适用场景**：生成配音、TTS、文字转语音、提取字幕、语音转文字、抠图、去背景

**冲突关系**：与 `hyperframes` 互斥（素材处理 vs 内容创作）

**配置文件**：[hyperframes-media/SKILL.md](skills/hyperframes-media/SKILL.md)

---

## 音乐处理类

### 18. pdf-to-musicxml-converter

**功能定位**：PDF 乐谱转 MusicXML 工具

**核心能力**：
- 光学乐谱识别（OMR）技术
- 支持钢琴谱、交响乐总谱、民乐乐谱
- 输出标准 MusicXML 4.0 格式

**适用场景**：PDF 转谱、乐谱转码、PDF 转 MusicXML、识别乐谱、数字化乐谱

**配置文件**：[pdf-to-musicxml-converter/SKILL.md](skills/pdf-to-musicxml-converter/SKILL.md)

---

## 技能列表汇总

| 序号 | 技能名称 | 分类 | 功能简述 |
|:---:|---------|------|---------|
| 1 | git-commit | 开发工具 | 规范代码提交 |
| 2 | skill-search-master | 开发工具 | 技能搜索安装 |
| 3 | ai-training-knowledge | AI 训练 | 大模型训练知识库 |
| 4 | frontend-skill | 前端开发 | 功能型前端构建 |
| 5 | frontend-design | 前端开发 | 创意型前端构建 |
| 6 | webapp-testing | 测试自动化 | 自动化测试 |
| 7 | data-analysis | 数据处理 | 数据分析与可视化 |
| 8 | canvas-design | 视觉设计 | 海报封面排版设计 |
| 9 | ai-image-generation | 视觉设计 | AI 生成式图片 |
| 10 | doc-coauthoring | 文档协作 | 文档协作编写 |
| 11 | brainstorming | 文档协作 | 需求方案设计 |
| 12 | general-web-crawler | 浏览器自动化 | 网页数据采集 |
| 13 | cybersecurity-audit-knowledge | 网络安全 | 安全审计知识库 |
| 14 | gsap | 动画特效 | GSAP 动画参考 |
| 15 | hyperframes | 视频制作 | HTML 视频合成 |
| 16 | hyperframes-cli | 视频制作 | HyperFrames 渲染工具 |
| 17 | hyperframes-media | 视频制作 | 素材预处理（TTS/转录/抠图） |
| 18 | pdf-to-musicxml-converter | 音乐处理 | PDF 乐谱转 MusicXML |

---

## 使用方式

### 自动调用

描述需求时，AI 会自动匹配并调用相关技能：

```
帮我分析这个 React 组件的性能问题
```

### 手动指定

明确指定要使用的技能：

```
用 git-commit 技能帮我提交代码
```

---

**更新时间**：2026-08-14（第五轮审计重建）

**技能总数**：19 个

---

*本文档由 Trae AI 自动生成，随 Skill 增删自动重建，保持与 `.trae/skills/` 目录一致。*
