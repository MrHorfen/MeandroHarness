# 鼓点识别主方案替换：HorfenDrums → HorfenDrumsHybrid（混合注意力）

- **状态**：implemented
- **日期**：2026-08-15
- **决策者**：用户（确认"直接替换主路径"+ 文档/Git 全面更新）
- **关联**：`AI乐谱大师/models/HorfenDrumsHybrid.py`、`AI乐谱大师/audio_transcription/drum_transcriber.py`、`AI乐谱大师/archive/HorfenDrums_v3.18/`、`AI乐谱大师/docs/technical/model_knowledge_base.md`

## 背景

原生产鼓转录模型 HorfenDrums（Conv+BiGRU+单层 Self-Attention）存在两个已验证问题：
1. **O(T²) 推理瓶颈**：Self-Attention 矩阵随帧数平方增长，12GB GPU 硬顶 ~170s（17000 帧），更长的歌被迫 30s 分块；分块拼接存在概率错位（TBD.md 实证：给地狱一点爱 148s 整曲拼接后 15301 帧 vs 真实 14801 帧，+500 帧错位；块边界概率偏差 ±99%）。
2. **欠召回**：全局阈值 0.95 极其保守，tom_high/tom_mid/ride 等稀疏类在验证集上几乎为 0（tom_high F1=0.0011）。

沙箱研发出 HorfenDrumsHybrid（3×GLA + 1×Anchor + AttnRes，SANA-Video 2.0 思路），GLA 线性注意力 O(T·d²) 支持整曲推理。控制变量测试发现：单曲上"效果更好"主因曾被误判为阈值，但**验证集帧级 F1（seed=42, val_ratio=0.2）证明架构收益是真实的**。

## 决策

1. **替换主路径**：`drum_transcriber.py` 的 NN 推理改为 HorfenDrumsHybrid 整曲推理 + 逐类校准阈值（0.5~0.7），method 标识 `nn_v3` → `nn_hybrid`；信号处理兜底保留。
2. **模型纳入生产**：`models/HorfenDrumsHybrid.py`（由沙箱 hybrid_model.py 逐字复制，架构零改动，推理逐位一致 max diff=0）+ `models/HorfenDrumsHybrid/horfen_drums_hybrid_best.pt` + `per_class_thresholds.json`。
3. **原方案归档**：`models/HorfenDrums.py` + `horfen_drums_best.pt` 归档至 `archive/HorfenDrums_v3.18/`（含回退说明）。
4. **版本升级**：v3.18.1 → v3.19.0，全量版本号 + 文档同步。

## 备选方案

1. **新分支并轨（method 参数切换）**：更保守，但用户选择直接替换（原方案已验证过时，保留双路径增加维护负担）。
2. **只改阈值不换架构**：控制变量测试证明不成立——生产模型在验证集上 0.95 已是最优阈值，降阈值伤 precision（0.1522 < 0.1683）；欠召回是架构能力上限。

## 影响

- 生产代码：`drum_transcriber.py`（`_load_model` 指向 HorfenDrumsHybrid、`_infer_nn` 整曲推理+逐类阈值、method 标识）
- 模型资产：新增 `models/HorfenDrumsHybrid/`，归档 `archive/HorfenDrums_v3.18/`
- 文档：model_knowledge_base（第17/19节）、business_overview、product_manual、architecture、README、CHANGELOG、project_rules 版本索引
- 版本号：launcher.py、backend main/monitor/system、frontend package.json/i18n

## 验证

- 生产 transcribe_drums 回归：HorfenDrumsHybrid 加载 ✓、逐类阈值生效 ✓、事件分布与沙箱校准版一致（kick 289/snare 280/hihat_closed 430/crash 40/tom_low 13）✓、method=nn_hybrid ✓
- 生产/沙箱推理逐位一致（max diff = 0）✓
- 验证集 F1：0.1793 → 0.2914（+62%），每类均提升
