# HorfenPiano v4 复兴训练方案（重启钢琴自研识别）

- **状态**：rejected（2026-08-30 被钢琴路线止损决策否决，见 `.trae/notes/implemented/process/2026-08-30-piano-route-stoploss.md`）
- **日期**：2026-08-19（提案）/ 2026-08-30（否决）
- **决策者**：用户（"及时止损不对的方向未必是坏事"）
- **关联**：`AI乐谱大师/temp/HorfenPiano_v4_复兴训练方案.md`（完整方案）、TBD.md §2.3（旧失败档案）、§2.7（止损结论）

## 否决理由（v4 segD 训完后获得的决定性证据）

1. **Phase A 门禁已实际不达标**：v4 完成 segA-D 四轮训练后，公平管线对比（v4 喂进与 Basic Pitch 完全同款的全套后处理）显示密度 15.3 音符/s vs Basic Pitch 5.2、max polyphony 16 vs 9，谱面质量仍差距明显；旋律召回天花板 64.4% 为帧级 BCE 架构固有，训练无法突破。
2. **"只换识别层、规训层原样保留"的前提崩塌**：HorfenScore 对 Basic Pitch 干净输出误删 63% 旋律音（负收益实锤），规训层与删除型训练配对深度绑定，不具备跨引擎复用性。
3. **止损判据**：自研训练只投给"开源做不了或做不好"的任务；钢琴恰是开源强项（Basic Pitch/MT3），solo 开发者在此训练 ROI 极低。
4. 提案中"新事实改变旧结论前提"的判断被证伪——旧结论（TBD §2.3"放弃自研钢琴识别"）依然成立，v4 是其第三次验证。

---

## 原提案内容（历史存档，仅作模式参考）

### 背景

用户决定重启钢琴识别模型训练（旧 HorfenPiano V1-V3 已于 2026-07-24 归档，结论"放弃自研"）。要求结合现有经验设计完整方案。

### 重启依据（新事实 vs 旧根因）

旧失败三大根因（数据不足 240 首伪标签 / 架构弱 / MuScriptor 标注噪声）在当前全部有解：MAESTRO v3（~200h 真实演奏 + Disklavier 物理真值 MIDI）+ HorfenDrumsHybrid 已验证的 GLA 骨架（验证集 F1 +62% 实证）。定位：**只换识别层，HorfenScore-Piano 写谱规训层原样保留**（与旧结论"写谱才是差异化"不冲突）。

### 决策（设计方案，四阶段门禁式）

- **Phase 0 沙箱准备**：MAESTRO 管线打通 + Basic Pitch 基线实测（同集 note-F1）+ 双头干扰预检（V3 教训）+ 长曲显存实测 + pos_weight 扫描
- **Phase A 基础训练**：MAESTRO 独奏全量，GLA 骨架 + 88 类双头（onset+frame），BCEWithLogitsLoss（FocalLoss 宪法禁令）。门禁：val note-F1 ≥ Basic Pitch 实测基线
- **Phase B 乐队适配**：污染训练（MAESTRO 钢琴 + Slakh2100 乐器轨混合，解 other stem 混乐器问题），lr=1e-5 微调 + KL 三级红线（0.05/0.10/0.20）+ identity_init=False 完整加载（鼓 Phase B 踩坑）
- **Phase C 生产集成**：88 类分区阈值校准（稀疏音区平滑）→ 端到端试听（用户耳朵验收）→ piano_transcribe.py 加分支 + Basic Pitch 永久回退（稳态优先）

**生产切换总门禁**：MAESTRO test F1 ≥ 基线 AND 污染集 F1 ≥ 基线-3% AND 用户试听认可；任一不过 → 生产维持 Basic Pitch，结论归档。

### 备选方案

1. **合成数据路线**（Salamander CC BY 音源 + 公有领域 MIDI）：零许可风险但 domain gap，仅在用户否决 MAESTRO NC 许可时启用
2. **维持现状（Basic Pitch 不自研）**：旧结论路线，未选——新事实（真值数据 + 已验证骨架）改变了旧结论的前提 ← **2026-08-30 复盘：此选项才是正确方向**

### 风险与止损（实际执行结果）

- 🟠 MAESTRO CC BY-NC-SA 4.0 非商用许可 → 实际执行了训练（个人研究用途），止损后训练集已删除
- 🟠 双头干扰复发（V3）→ segA-D 采用分段训练规避，但架构错配未解
- 止损条款：Phase A 两轮迭代不达基线 → 承认失败归档，不无限投入 ← **2026-08-30 触发：正式归档止损**

### 影响

- 沙箱：temp/ 下新增方案文档；执行时 MAESTRO 数据 + 训练产物约 25-30GB 磁盘 ← **实际膨胀至 216GB，止损时删除**
- 生产：方案批准并走完 Phase C 前零影响 ← **实际：生产始终维持 Basic Pitch，零影响兑现**
