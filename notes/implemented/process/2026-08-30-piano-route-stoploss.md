# 钢琴路线止损：停止 HorfenPiano/HorfenScore 训练投入，生产维持 Basic Pitch

- **状态**：implemented
- **日期**：2026-08-30
- **决策者**：用户（"及时止损不对的方向未必是坏事"；确认全删 216GB 训练集 + 升 v3.22.0）
- **关联**：`.trae/notes/rejected/process/2026-08-19-horfenpiano-v4-revival.md`（本决策否决该提案）、`AI乐谱大师/TBD.md §2.3/§2.7`、`AI乐谱大师/docs/technical/model_knowledge_base.md §12/§13`、`AI乐谱大师/archive/HorfenPiano_v4/`、`AI乐谱大师/archive/HorfenScore_v7.5/`

## 背景

v4 复兴训练（segA-D 四轮）完成后，用户以"写谱质量"为目标做迭代验收（C1 Popular + Summer 双曲），沙箱实验链（差距分析→v2b 规则→生产后处理组合→规训交叉验证→公平管线消融→音频预处理消融）得出决定性证据：

1. **v4 帧级 BCE 架构与"准确且干净"目标根本错配**：密度 26 音符/s（人类真值 ~10）、max polyphony 27、旋律召回天花板仅 64.4%（raw 阶段固有）。4 轮训练 + 4 版清理规则（v1/v2/v2b/v3）+ 全套生产后处理仍达不到 Basic Pitch 开箱即用的谱面质量。
2. **HorfenScore 规训与 Basic Pitch 输出不兼容**：训练配对是"v4 脏→干净"，删除判据只对 v4 式噪声有效；接在 C 输出上误删 63% 最高声部（旋律音）+ 10 处整簇和弦被端掉——纯负收益。
3. **音频预处理救不了 Basic Pitch 和弦漏检**（消融证伪"预处理>训练"假设的音高端推广）：HPSS 谐波/低频 EQ/带通三变体对 F 方案 361 处 ≥4 音和弦时刻的补全率仅 6-12%（raw 基线 11%）。和弦漏检是 note-level decoder 架构特性，不是信号质量问题。
4. **训练集无罪**：prepare_maestro_v4.py 直读 MAESTRO 官方干净 WAV，无 L2 归一化；训练/推理 mel 口径逐参数一致。问题在架构选型，不在数据污染。
5. **规训生产零接线确认**：全项目 Grep 证实 `HorfenScorePostprocessor` 无任何生产调用点，"生产不加 horfenscore"无需改代码。
6. **附带发现的真实生产 bug**：`preprocess_audio_for_transcription` 的 L2 归一化（÷RMS）在大动态乐曲上放大 ~10 倍致 24.6% 削波；且 BeatTracker 在干净信号上对 6/8 抒情曲判 BPM=83.4（小节层级）、在削波信号上判 123.0（八分层级）——生产节拍检测实际依赖失真信号，属 🔴 待修复缺陷（记入 TBD）。

## 决策

1. **停止 HorfenPiano（v4）与 HorfenScore 的一切训练投入**，两模型降级为"已有资产"归档保留（权重+训练记录+脚本），不再迭代。
2. **生产钢琴链路维持 Basic Pitch 现状**（本就零规训接线，代码零改动）。
3. **删除 216GB 训练集**：`temp/maestro-v3.0.0/`（129.05GB，官方可重下）+ `temp/horfenpiano_v4/preprocessed/`（87.22GB，可重生成）。分批执行、回收站方式、验证留痕。
4. **v4 复活提案（2026-08-19）转 rejected**：其 Phase A 门禁（val note-F1 ≥ Basic Pitch 基线）在 v4 segD 上已被验证不达标，且"准确干净"目标下帧级 BCE 路线整体不成立。
5. **后续投入方向**：①音频预处理攻坚（重点修 L2 削波 bug + 节拍层级策略）；②零训练规则工程（如 C 旋律 + v4 和弦补全的融合方案，作为可选增强，不强制落地）；③其他乐器训练继续（鼓/贝斯/小提琴路线不受影响）。
6. **止损判据沉淀为项目原则**：自研训练只投给"开源做不了或做不好"的任务。鼓（无开源方案+架构匹配）成功、钢琴（Basic Pitch/MT3 生态成熟+架构错配）失败，正反两面验证了该判据。
7. **版本升级** v3.21.1 → v3.22.0（战略级更新，文档/资产/记忆全量同步）。

## 备选方案（均已否决）

1. **继续训练 v4（原提案 Phase B/C）**：帧级 BCE 的高召回特性与"准确干净"目标对抗，solo 开发者训练 ROI 极低。
2. **和弦补全融合方案立即落地**：用户判定"4 没必要"——融合规则（C 全保留+v4 补和弦）沙箱已验证数据面成立（92% 缺口定位），但不进入本轮生产，留 TBD 可选方向。
3. **保留训练集以备重训**：216GB 占用过大，MAESTRO 可重下、preprocessed 可重生成，保留价值低于磁盘成本。

## 影响

- 生产代码：零改动（规训本就未接线）。
- 资产：`archive/HorfenPiano_v4/`（ckpt+训练记录+脚本清单）、`archive/HorfenScore_v7.5/`（ckpt+模块说明）；删除 216GB 训练数据。
- 文档：TBD.md（§2.3 更新 + 新增 §2.7 止损结论 + L2 削波 bug 条目）、model_knowledge_base §12/§13、business_overview、training_log、CHANGELOG、README。
- 版本：launcher/backend/main/system/monitor/frontend package.json/i18n（简繁英）→ 3.22.0。
- 记忆：project_memory.md 记录止损判据、L2 削波 bug、规训零接线事实。
- 知识产权证据：_ip_vault 补充 v4/HorfenScore 训练成果归档记录（模型与训练日志保留，删除的仅为可再生数据）。

## 验证

- 全项目 Grep 确认 HorfenScorePostprocessor 零生产调用 ✓（决策前置勘察）
- 删除后 `temp/maestro-v3.0.0`、`temp/horfenpiano_v4/preprocessed` 不存在，其他乐器训练数据（drums/bass_distill/violin_distill）完好 ✓（删除执行时逐项验证）
- 版本号 grep 一致性审查（11 处 3.22.0，无 3.21.1 残留）✓（版本更新时执行）
- 归档 ckpt 可加载验证（torch.load）✓（归档时执行）
