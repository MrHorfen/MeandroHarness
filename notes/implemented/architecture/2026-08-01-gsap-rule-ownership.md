# GSAP 动画规则唯一 owner 归属

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：master-controller
- **关联**：`gsap-animation-master/AGENT.md`、`hyperframes-video-producer/AGENT.md`

## 背景

`gsap-animation-master` 和 `hyperframes-video-producer` 两个 Agent 各自定义了 GSAP 动画规则（如时间线 paused、禁止随机函数、禁止 CSS transition 混用等），导致 12 条规则在两处重复维护。修改时可能不同步，造成 🟠 设计风险。

## 决策

**唯一 owner 原则**：`gsap-animation-master` 为 GSAP 规则的唯一 owner。

`hyperframes-video-producer` 不再重复定义 GSAP 规则，改为引用 `gsap-animation-master`。仅保留 4 条与 HyperFrames 渲染直接相关的核心速查（时间线 paused、禁止随机函数、渲染验证、截图验证），其余 8 条指向 `gsap-animation-master`。

## 备选方案

1. **hyperframes-video-producer 为唯一 owner**：不选。GSAP 是动画领域知识，hyperframes 是渲染引擎，职责不同
2. **两者各自维护**：不选。这正是要去重的问题

## 影响

- `hyperframes-video-producer/AGENT.md`：12 条规则 → 4 条核心速查 + 明确引用
- `gsap-animation-master/AGENT.md`：增加"唯一 owner"标注
- 后续任何 GSAP 规则变更只需改一处