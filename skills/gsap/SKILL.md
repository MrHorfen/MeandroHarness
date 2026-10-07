---
name: "gsap"
description: "GSAP 动画库参考与代码生成。用于 HyperFrames 合成中的时间线动画、缓动函数、入场出场效果、滚动触发等。触发：用户要「加动画」、「GSAP动画」、「入场效果」、「转场动画」、「时间线动画」、「缓动效果」。不触发：纯CSS动画→直接写CSS；视频整体策划→用media-producer。"
conflicts_with: ["hyperframes"]
---

# GSAP 动画技能

GSAP 动画库参考与代码生成，用于 HyperFrames 合成和 Web 项目中的动画效果。

## 何时触发

✅ **WHEN TO USE:**
- 用户要「加动画」、「做动画效果」、「GSAP 动画」
- 用户要入场动画、出场动画、转场过渡
- 用户要时间线动画（多元素序列动画）
- 用户提到缓动函数、easing、stagger
- HyperFrames 项目需要添加动画效果
- 用户要滚动触发动画（scroll-trigger）
- 用户要 SVG 动画、文字动画

❌ **WHEN NOT TO USE:**
- 纯 CSS 动画（简单的 hover、transition）→ 直接写 CSS
- 完整视频项目策划 → 用 media-producer
- 纯 Three.js/WebGL 动画 → 其他专门工具

⚠️ **EDGE CASES:**
- HyperFrames 项目里加动画 → 与 hyperframes skill 配合使用
- 场景转场动画 → 用 hyperframes skill 整体编排，gsap skill 写具体动画代码

## 核心 API

### 基础动画
```javascript
// 到目标状态
gsap.to(".element", { x: 100, opacity: 1, duration: 1 })

// 从起始状态
gsap.from(".element", { y: 50, opacity: 0, duration: 0.8 })

// 从到
gsap.fromTo(".element", 
  { scale: 0 }, 
  { scale: 1, duration: 1 }
)

// 设置初始状态
gsap.set(".element", { opacity: 0 })
```

### 时间线（Timeline）
```javascript
const tl = gsap.timeline({ paused: true })

tl.to(".el1", { x: 100, duration: 1 })
  .to(".el2", { y: 50, duration: 0.8 }, "-=0.3")  // 提前0.3s开始
  .to(".el3", { opacity: 1, duration: 0.5 }, "+=0.2")  // 延后0.2s
```

### 常用缓动函数
| 缓动 | 效果 | 适用场景 |
|------|------|---------|
| `power1.out` | 轻微减速 | 通用 |
| `power2.out` | 自然减速 | 入场动画 |
| `power3.out` | 明显减速 | 强调元素 |
| `power4.out` | 强烈减速 | 重磅出场 |
| `back.out(1.7)` | 轻微回弹 | 弹性入场 |
| `elastic.out(1, 0.3)` | 弹性效果 | 强调效果 |
| `bounce.out` | 弹跳效果 | 趣味场景 |
| `expo.out` | 极快减速 | 快速入场 |
| `sine.out` | 平滑正弦 | 柔和过渡 |

### Stagger（交错动画）
```javascript
gsap.from(".items", {
  y: 30,
  opacity: 0,
  duration: 0.6,
  stagger: 0.1,  // 每个元素间隔0.1s
  ease: "power2.out"
})
```

## HyperFrames 中的 GSAP 规范

### 必须遵守的规则
1. 所有时间线必须 `{ paused: true }`
2. 必须注册到 `window.__timelines["composition-id"]`
3. 禁止 `Math.random()`、`Date.now()` 等随机/时间函数
4. 禁止 `repeat: -1`（无限循环）
5. 只允许动画属性：opacity、x、y、scale、rotation、color、backgroundColor
6. 禁止动画 visibility、display
7. 禁止异步构建时间线（禁止 async/setTimeout/Promise）
8. 入场动画偏移量：`0.1s ~ 0.3s`
9. 每个 composition 至少使用 3 种不同缓动函数
10. 禁止在不同时间线上动画同一元素的同一属性

### HyperFrames 标准写法
```javascript
window.addEventListener("DOMContentLoaded", () => {
  const comp = document.querySelector(".composition")
  const compId = comp.dataset.compositionId || "main"

  const tl = gsap.timeline({ paused: true })

  tl.from(".title", {
    y: -40,
    opacity: 0,
    duration: 0.8,
    ease: "power3.out"
  })
  .from(".subtitle", {
    y: 20,
    opacity: 0,
    duration: 0.6,
    ease: "power2.out"
  }, "-=0.4")

  window.__timelines = window.__timelines || {}
  window.__timelines[compId] = tl
})
```

## 与其他 Skill 的配合

| 场景 | 配合 Skill |
|------|-----------|
| HyperFrames 视频项目 | hyperframes + hyperframes-cli |
| 前端页面动画 | frontend-skill / frontend-design |
| SVG 动画 | canvas-design |
