---
name: "hyperframes-media"
description: "HyperFrames 素材预处理工具。支持文本转语音（TTS配音）、音频/视频转录（Whisper字幕）、背景移除（透明化）。触发：用户要「生成配音」、「TTS」、「文字转语音」、「提取字幕」、「语音转文字」、「抠图」、「去背景」。不触发：完整视频制作→用hyperframes或media-producer。"
conflicts_with: ["hyperframes"]
---

# HyperFrames Media 技能

HyperFrames 素材预处理——文本转语音配音、音视频转录字幕、背景移除。

## 何时触发

✅ **WHEN TO USE:**
- 用户要「生成配音」、「TTS」、「文字转语音」
- 用户要「提取字幕」、「语音转文字」、「自动打轴」
- 用户要「抠图」、「去背景」、「透明化」
- 用户说「给这段文字配个音」
- 用户说「把这个视频的台词提取出来」
- 用户说「把这张图的背景去掉」
- HyperFrames 项目需要配音或字幕素材

❌ **WHEN NOT TO USE:**
- 完整视频策划与制作 → 用 media-producer 或 hyperframes
- 通用 ffmpeg 音视频处理 → 用 media-producer
- 视频渲染 → 用 hyperframes-cli

⚠️ **EDGE CASES:**
- 用户说「给视频加字幕」→ 如果是 HyperFrames 项目，先 transcribe 提取字幕，再集成到项目中
- 用户说「做个带配音的视频」→ 先生成配音（本技能），再创建 HyperFrames 项目（hyperframes skill），最后渲染（hyperframes-cli）

## 核心能力

### 1. 文本转语音（TTS 配音）
- 引擎：Kokoro
- 支持中英文
- 输出：音频文件 + 字级时间戳（用于字幕同步）

命令：
```bash
# 生成配音
npx hyperframes tts "文本内容" --output audio.wav --voice zh-CN-XiaoxiaoNeural

# 从文件生成
npx hyperframes tts -f script.txt --output audio.wav
```

### 2. 音视频转录（生成字幕）
- 引擎：Whisper
- 支持音频和视频输入
- 输出：SRT/VTT 字幕文件 + 转录文本

命令：
```bash
# 从音频生成字幕
npx hyperframes transcribe audio.wav --output subtitles.srt

# 从视频提取字幕
npx hyperframes transcribe video.mp4 --output subtitles.srt

# 选择模型（默认 small）
npx hyperframes transcribe input.mp3 --model base
```

### 3. 背景移除
- 引擎：u2net
- 支持图片和视频
- 输出：透明背景的 PNG / WebM

命令：
```bash
# 图片去背景
npx hyperframes remove-background input.jpg --output output.png

# 视频去背景
npx hyperframes remove-background input.mp4 --output output.webm
```

## 中文语音选项

| 语音 ID | 风格 | 适用场景 |
|---------|------|---------|
| `zh-CN-XiaoxiaoNeural` | 女声，自然 | 通用讲解、旁白 |
| `zh-CN-YunxiNeural` | 男声，自然 | 正式讲解、新闻 |
| `zh-CN-XiaoyiNeural` | 女声，活泼 | 短视频、年轻化 |

## 典型工作流

### 配音 + 字幕一条龙
```
1. 准备文案 script.txt
2. hyperframes tts -f script.txt --output audio.wav → 生成配音+时间戳
3. hyperframes transcribe audio.wav --output subtitles.srt → 生成字幕（或直接用 TTS 返回的时间戳）
4. 将音频和字幕集成到 HyperFrames 项目
5. hyperframes render → 最终视频
```

## 注意事项

- 首次运行会自动下载模型（Kokoro/Whisper/u2net），需要网络
- Whisper 模型越大准确率越高但速度越慢：tiny < base < small < medium < large
- 背景移除的视频输出格式是 WebM（支持透明通道）
