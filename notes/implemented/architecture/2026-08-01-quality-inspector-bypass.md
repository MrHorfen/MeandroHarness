# quality-inspector 越级调度设计

- **状态**：implemented
- **日期**：2026-08-01
- **决策者**：master-controller
- **关联**：`quality-inspector/AGENT.md §校验失败后的协作链`

## 背景

`quality-inspector` 在校验失败后直接调度 `fullstack-code-developer` 和 `iteration-fallback` 进行纠偏，绕过了 `master-controller`。这与"master-controller 是唯一调度中枢"的原则冲突，存在两个调度者同时操作同一 Agent 的风险。

## 决策

**保留越级调度，但加互斥约束**：

1. 越级调度是刻意设计——校验→纠偏→再校验的闭环必须在最短路径内完成，避免调度延迟导致上下文丢失
2. 加互斥锁：调度前必须检查目标 Agent 是否已被 master-controller 占用；若冲突，等待或上报 master-controller 协调

## 备选方案

1. **所有纠偏都经 master-controller 中转**：不选。增加延迟，且 master-controller 不了解校验上下文的细节，中转会丢失信息
2. **quality-inspector 仅报告不纠偏**：不选。会导致"发现问题→等总控→重新调度→再次校验"的冗长循环

## 影响

- `quality-inspector/AGENT.md` 增加设计说明 + 互斥约束段
- 这是宪法级"唯一调度中枢"原则的例外，需要显式标注