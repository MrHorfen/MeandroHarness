# 鼓点风格渐进加深训练方案（Phase B 沙箱验证通过）

- **状态**：proposed（沙箱验证完成，生产替换待数据补强 + 用户确认）
- **日期**：2026-08-15
- **决策者**：用户（确认"先写方案文档+沙箱验证"）
- **关联**：
  - `AI乐谱大师/temp/风格渐进加深训练方案.md`（完整方案文档）
  - `AI乐谱大师/temp/scripts/drums_style_weighted_train.py`（沙箱逐风格加权训练脚本）
  - `AI乐谱大师/temp/training/drums_style_weighted/training_report.json`（Phase B 验证报告）
  - `AI乐谱大师/models/HorfenDrumsHybrid/`（生产主模型，替换前提）

## 背景

用户反馈鼓点识别"风格识别欠缺，需缓慢加深训练"。实测瓶颈根因：

1. **风格数据不均衡**（`temp/training_data/drums_annotated/` 263 首）：rock 86 / metal 67 / jazz 63 / fusion_jazz 47
2. **难度不均**：jazz 是历史最难 F1，fusion_jazz 数据量最少 → 双重短板

目标：在不遗忘原风格前提下，渐进提升 jazz/fusion_jazz 识别能力。

## 决策

采用**三阶段渐进加深路线**（详见方案文档）：

- **Phase A 数据补强**：扩充 jazz/fusion_jazz（源数据补标 + 合成密集扩增 + 温和增强），收窄不均衡到 ≤1.3 倍
- **Phase B 稳定底座**：生产 hybrid 权重上 lr=1e-5 微调，逐风格加权采样，KL ≤0.05 红线，确认不遗忘
- **Phase C 风格定向加深**：多轮"补数据→微调→按风格 F1 验收→阈值重校准→试听→替换生产"

### 沙箱改造（drums_style_weighted_train.py）

- 新增 `--style-weights` 逐风格加权采样（rock:1.0 / jazz:1.25 / fusion_jazz:1.35 / metal:1.15，数量反比+难度系数）
- **关键修复**：从生产 hybrid 权重微调必须 `identity_init=False` + 完整 `load_state_dict`（原 `load_pretrained_weights` 是"旧 HorfenDrums→hybrid"迁移专用，键名不匹配导致 48 层未加载、F1=0）

## 备选方案

1. **一次性大改（更多数据一次性训练）**：违反"缓慢加深、可验证"原则，遗忘风险高，未选
2. **只加权重不补数据**：Phase B 实证证明——现有数据量下逐风格加权对 jazz 无实质提升（jazz F1 0.2205→0.2186 微降），数据量不足才是主因

## 影响

- 沙箱：新增 `drums_style_weighted_train.py` + `temp/training/drums_style_weighted/` 训练产物
- 生产：暂不影响（替换前提 = Phase A 数据补强 + Phase C 验收通过 + 用户确认）
- 训练数据：`temp/training_data/drums_annotated/` 为核心资产，继续扩充

## 验证（Phase B 沙箱结果）

对比生产基线（epoch 0）vs 加权训练 best（epoch 2，seed=42/val_ratio=0.2）：

| 指标 | 生产基线 | 加权 best | 变化 |
|------|---------|----------|------|
| overall F1 | 0.2654 | 0.2662 | +0.3% |
| dense recall | 0.4186 | 0.4347 | **+3.8%** |
| rock F1 | 0.2612 | 0.2647 | +1.3% |
| jazz F1 | 0.2205 | 0.2186 | -0.9% |
| fusion_jazz F1 | 0.2805 | 0.2803 | ~0 |
| metal F1 | 0.2993 | 0.3012 | +0.6% |
| KL 稳定性 | - | ≤0.0011 | 极稳定（红线 0.05） |

**结论**：
1. ✅ **稳定底座目标达成**：KL 全程 ≤0.0011，整体 F1 不降，rock/metal 保护组无遗忘
2. ✅ **dense recall 提升**：+3.8%（密集击打识别改善）
3. 🟡 **风格定向未达预期**：jazz 微降、fusion_jazz 持平 → 印证"数量不均衡是主因"，**必须走 Phase A 补数据**才能实质提升
4. 生产模型**不替换**，等待 Phase A 数据补强后推进 Phase C

## 下一步

1. Phase A：扩充 jazz/fusion_jazz 数据（源数据补标/合成扩增），收窄不均衡
2. Phase C：多轮定向加深，按风格 F1 门禁验收
3. 每轮阈值重校准 → 沙箱试听 → 用户确认 → 替换生产
