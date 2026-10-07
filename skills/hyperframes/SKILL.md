---
name: "hyperframes"
description: "创建和编辑 HyperFrames HTML 视频合成项目。支持视频合成、动画、字幕、配音、音频可视化、场景转场等所有 HyperFrames 内容创作。触发：用户要「做个视频」、「HyperFrames项目」、「HTML视频」、「动画视频」、「加字幕」、「做个片头」。不触发：只是渲染已有项目→用hyperframes-cli；只是写剧本策划→用media-producer。"
conflicts_with: ["hyperframes-cli"]
---

# HyperFrames 视频合成技能

在 HyperFrames HTML 中创建视频合成、动画、字幕、配音、音频响应式视觉、场景转场等内容。

## 何时触发

✅ **WHEN TO USE:**
- 用户要创建新的 HyperFrames 视频项目
- 用户要「做个视频」、「做个动画视频」、「HTML 视频」
- 用户要加字幕、配音、转场效果
- 用户要做音频响应式视觉（音乐可视化）
- 用户要做片头、片尾、LOGO 动画
- 用户提到 HyperFrames、hyperframes、视频合成
- 用户要修改现有 HyperFrames 项目的内容

❌ **WHEN NOT TO USE:**
- 只是渲染已有项目为 MP4 → 用 hyperframes-cli
- 只是写视频剧本/策划 → 用 media-producer
- 只是生成 TTS 配音 → 用 hyperframes-media
- 纯 GSAP 动画代码 → 用 gsap skill

⚠️ **EDGE CASES:**
- 用户说「帮我做个 HyperFrames 视频」→ 先策划（media-producer）→ 再创建项目（hyperframes）→ 最后渲染（hyperframes-cli）
- 用户说「给这个项目加点动画」→ hyperframes + gsap 配合

## 核心能力

### 视频合成
- 创建完整的 HyperFrames 合成项目（index.html）
- 多场景时间线编排（data-start / data-duration）
- 多轨道分层（track-index）
- 场景转场过渡

### 内容类型
- 标题卡片、字幕卡片
- 图文排版布局
- 数据可视化展示
- 音乐可视化（音频响应）
- 产品介绍、科普讲解

### 动画能力
- GSAP 时间线动画
- 入场/出场动画
- 过渡效果（fade、slide、scale、wipe）
- 音频响应式动画（beat sync）

### 字幕与配音
- 字幕同步显示
- TTS 配音集成
- 逐字高亮动画

## 项目结构规范

```
project-dir/
├── index.html          # 主合成文件
├── audio.wav           # 配音/BGM（可选）
├── subtitles.srt       # 字幕文件（可选）
└── assets/             # 图片等素材（可选）
```

## HTML 结构规范

- 根元素：`<div class="composition" data-width="1920" data-height="1080">`
- 场景：`<div class="clip scene" data-start="0" data-duration="5" data-track-index="0">`
- 同一 track-index 的片段时间不能重叠
- 每两个相邻场景必须有过渡动画

## 尺寸规范
- 横版 16:9：1920 × 1080
- 竖版 9:16：1080 × 1920
- 方形 1:1：1080 × 1080

## 排版规范
- 全局字体：`"Microsoft YaHei", "PingFang SC", sans-serif`
- 标题 72px+，正文 28px+
- Flex 布局为主，内容容器禁止绝对定位
- 禁止用 `<br>` 强制换行，用 max-width 自然折行

## 与其他 Skill 的配合

| 任务阶段 | 使用 Skill |
|---------|-----------|
| 剧本策划 | media-producer |
| TTS配音/转录 | hyperframes-media |
| 创建/编辑项目 | hyperframes（本技能） |
| GSAP动画细节 | gsap |
| 渲染MP4 | hyperframes-cli |
| Registry组件 | hyperframes-registry |
