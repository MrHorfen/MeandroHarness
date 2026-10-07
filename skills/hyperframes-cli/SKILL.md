---
name: "hyperframes-cli"
description: "HyperFrames CLI 开发循环工具。用于项目脚手架初始化（init）、代码校验（lint）、结构验证（inspect/validate）、预览（preview）、渲染MP4（render）、环境排查（doctor）。触发：用户要「渲染视频」、「导出MP4」、「hyperframes render」、「lint检查」、「预览项目」。不触发：创建或编辑项目内容→用hyperframes；视频策划→用media-producer。"
conflicts_with: ["hyperframes"]
---

# HyperFrames CLI 技能

HyperFrames 命令行开发循环工具——脚手架初始化、代码校验、结构验证、预览、渲染、环境排查。

## 何时触发

✅ **WHEN TO USE:**
- 用户要「渲染视频」、「导出 MP4」、「生成视频」
- 用户说「hyperframes render」、「渲染这个项目」
- 用户要 lint 检查项目错误
- 用户要预览 HyperFrames 项目
- 用户要初始化新的 HyperFrames 项目脚手架
- 用户遇到 HyperFrames 环境问题需要排查
- 任何涉及 `hyperframes` CLI 命令的操作

❌ **WHEN NOT TO USE:**
- 创建或编辑项目的 HTML/动画内容 → 用 hyperframes
- 视频剧本策划、创意构思 → 用 media-producer
- 生成 TTS 配音 → 用 hyperframes-media
- 安装 HyperFrames 本身 → 用 npm/pnpm

⚠️ **EDGE CASES:**
- 用户说「帮我把这个做成视频」→ 先确认有没有 HyperFrames 项目，没有的话先用 hyperframes skill 创建
- 渲染失败时 → 先 lint，再 doctor，最后再尝试渲染

## 核心命令

| 命令 | 用途 | 典型场景 |
|------|------|---------|
| `npx hyperframes init <dir>` | 初始化新项目 | 从零创建脚手架 |
| `npx hyperframes lint <dir>` | 检查代码错误 | 渲染前必做 |
| `npx hyperframes inspect <dir>` | 检查合成结构 | 验证时间线 |
| `npx hyperframes validate <dir>` | 深度校验项目 | 完整结构验证 |
| `npx hyperframes preview <dir>` | 浏览器预览 | 实时查看效果 |
| `npx hyperframes render <dir> -o <output.mp4>` | 渲染MP4 | 最终导出 |
| `npx hyperframes doctor` | 环境诊断 | 环境出问题时 |
| `npx hyperframes upgrade` | 升级版本 | 版本更新 |

## 渲染参数

```bash
# 基础渲染
hyperframes render <project-dir> --output <output.mp4>

# 高质量渲染
hyperframes render <project-dir> --output <output.mp4> --quality high

# 指定帧率
hyperframes render <project-dir> --output <output.mp4> --fps 30
```

## 标准工作流

```
1. hyperframes lint project-dir/   # 先检查错误
2. 修复 lint 报错
3. hyperframes validate project-dir/  # 深度校验（可选）
4. hyperframes preview project-dir/   # 预览确认
5. hyperframes render project-dir/ -o output.mp4 --quality high  # 最终渲染
```

## 常见问题排查

| 问题 | 排查步骤 |
|------|---------|
| lint 报错 | 看行号，检查 data-* 属性、HTML 语法 |
| 渲染黑屏 | 检查 data-start / data-duration，确保场景有内容 |
| 动画不播放 | 检查 GSAP 时间线是否 paused: true，是否注册到 window.__timelines |
| 环境报错 | 先跑 hyperframes doctor |
| 渲染慢 | 60秒视频约2-5分钟属正常，可用 --quality medium 加速 |
