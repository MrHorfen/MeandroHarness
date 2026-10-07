---
name: "ai-image-generation"
description: "通过 AI 模型（FLUX/Gemini/Grok/Seedream 等）生成照片级写实图片、概念图、艺术图。触发：用户要「生成图片」、「AI画图」、「写实照片」、「概念艺术」、「AI生图」。不触发：用户要「设计海报」、「排版封面」、「配色构图」→ 用 canvas-design。"
conflicts_with: ["canvas-design"]
---

# AI Image Generation 技能

通过 AI 模型（FLUX Dev LoRA, FLUX.2 Klein LoRA, Gemini 3 Pro Image, Grok Imagine, Seedream 4.5, Reve 等）生成照片级写实图片、概念图、艺术图。

## 何时触发

✅ **WHEN TO USE:**
- 用户要「生成一张照片级图片」、「AI画图」、「AI生图」
- 用户要「写实风格」、「概念艺术」、「插画」、「产品图」
- 用户明确提到 flux, image generation, ai image, text to image, stable diffusion
- 用户需要 AI 模型生成的图片内容（照片、插画、艺术创作）

❌ **WHEN NOT TO USE:**
- 用户要「设计」海报、封面等排版型视觉内容 → 用 canvas-design
- 用户要「配色构图」、「排版设计」→ 用 canvas-design
- 用户要代码输出的图形设计 → 用 canvas-design
- 用户要数据图表 → 用 data-analysis（已吸收 chart-visualization）

⚠️ **EDGE CASES:**
- 用户说「生成一张海报图」→ 先确认：是 AI 照片级生成（ai-image-generation）还是代码排版设计（canvas-design）
- 关键词含「照片」「写实」「像真的一样」→ ai-image-generation；含「设计」「排版」「配色」→ canvas-design

## 功能说明

### 支持的模型
- FLUX Dev LoRA
- FLUX.2 Klein LoRA
- Gemini 3 Pro Image
- Grok Imagine
- Seedream 4.5
- Reve, ImagineArt 等 50+ 模型

### 能力
- 文生图（text-to-image）
- 图生图（image-to-image）
- 图像修复（inpainting）
- LoRA 微调
- 图像编辑
- 超分辨率放大
- 文字渲染

### 使用场景
- AI 艺术创作
- 产品效果图
- 概念图/原型图
- 社交媒体图形
- 营销视觉素材
- 插图生成

## 注意事项
- 通过 inference.sh CLI 调用
- 不同模型适合不同风格，需根据需求选择
- 生成结果可能受模型训练数据影响