# EACL 2027 投稿合规清单与待办

> 依据：https://2027.eacl.org/calls/papers/、https://aclrollingreview.org/cfp 与 https://acl-org.github.io/ACLPUB/formatting.html（2026-07-29 复核）。投稿路径：**ARR（OpenReview）→ EACL 2027 承诺（commitment）**。
> 本文件 = 投稿工作总账。结构：① 待办 ② 构建状态 ③ 日期/合规/Checklist ④ **生效红线速查（改论文前必读、勿回改）** ⑤ 已解决历史归档。

## 🔴 投稿前待办（TODO，按优先级）

1. **OpenReview/ARR 投稿表单**：Responsible NLP checklist（草稿见 §Checklist）、preprint/匿名选择、AI 写作披露、作者名单与适用时的 resubmission 信息；ARR 提交后作者名单不可随意变更。
2. **全体作者 ARR 审稿注册**（2026-08-05 EoD AoE 前，强制）。
3. **Mac 本地重编译终检**（07-29 沙盒已按最新 tex 以 Times New Roman 全量重编译并核实：正文 §1–Conclusion 完整止于 p.8，Limitations 同页开始；引言/相关工作压缩后已留出明显分页余量；`tab:lang-quadrant` 位于附录 p.19、仍在 C.7 内；无 overfull/未定义引用）。Mac 重编译后再跑 `pdffonts` 确认主字体为 TimesNewRomanPSMT，并复核分页未因本地字体环境变化而漂移。
4. **数据许可（2026-07-21 已核对可见发布条件，见 §Checklist "五源许可"表）**：iSarcasm/CSC 的源仓库声明 MIT；ToSarcasm、News Headlines 与 Open Chinese 的所访问版本未识别到明确再分发许可。后三者不再分发人类原文；DualTest 仅发布具备发布条件的 AI 侧、标签/元数据及要求用户从原始来源取得人类语料的重建脚本。剩：① News Headlines 的 Kaggle license 字段投稿前最后核实一次；② 匿名代码/数据用 Anonymous GitHub（禁 Dropbox/Drive）。
5. **两阶段提交**：2026-08-03 23:59 AoE 前在 OpenReview 提交 ARR 2026-08 周期；取得完整 reviews/meta-review 后，于 2026-10-11 23:59 AoE 前另行提交 EACL 2027 commitment 表单。

（原"英译""ACL 模板排版""文献核实""中文示例拼音转写""aclpubcheck/pdffonts/匿名化/元数据终检"已完成，见构建状态与 07-15、07-28、07-29 归档。）

### ✅ 文献核实结果（07-15 初核；07-28 全表审计；07-29 正式版本复核，custom.bib 已修）

- **全表审计（07-28）** ✅ 68 个被引用 key 与 68 个 bib 条目一一对应，无缺失、冗余或重复 key；其中 30 条已对 ACL Anthology、出版社、OpenReview、PMLR、CVF、DOI 或官方模型页面逐字段复核，其余为领域标准条目；BibTeX/LaTeX 无未定义引用。

- **Bevendorff 2025** ✅ 与 ACL Anthology 完全一致（Findings ACL 2025, pp.3762–3787, doi:10.18653/v1/2025.findings-acl.194）；正文"frame as authorship verification"表述与摘要相符。
- **Chai et al. 2025** ✅ CVPR 2025 openaccess 完全一致（Chai, Lu, Wang, pp.25698–25707）。
- **Compton 2023** 🔧 已修：venue 原写 CHIL，实为 *Proceedings of the 8th Machine Learning for Healthcare Conference*（PMLR 219），补 pages 110–127、publisher PMLR。
- **Du 2022（PALI-NLP）** 🔧 已修：作者原列 6 人（含 Jin Meizhi、Mo Yang），Anthology 正典为 5 人：Du, Hu, **Zhi (Jin)**, Jiang, Shi。
- **Li 2023** ✅ EMNLP 2023 完全一致（pp.16460–16476）；补 doi。
- **ViSP** ✅ 期刊版确认：*Neurocomputing* 678:133185, doi:10.1016/j.neucom.2026.133185，作者 Changli Wang, Fang Yin, Jiafeng Liu, Rui Wu（arXiv 版仅 3 作者，期刊版含 Jiafeng Liu，勿按 arXiv 改）。
- **SarcNet（Yue 2024）** ✅ LREC-COLING 2024 完全一致（Yue, Shi, Mao, Hu, Cambria, pp.14325–14335）。
- **Ye** 🔧 已更新为正式版：*The Clever Hans Mirage: A Comprehensive Survey on Spurious Correlations in Machine Learning*, **TMLR 2026**（OpenReview kIuqPmS1b1），作者扩至 19 人；bib key `ye2024spurious` 不变，渲染变为 "Ye et al., 2026"。
- **DeepSeek-R1** 🔧 07-29 按 ARR“预印本已有正式同行评审版本时应引用正式版”的要求，将 `deepseek2025r1` 从 arXiv 条目更新为 Nature 正式版：Guo, Yang, Zhang et al., *Nature* 645:633–638 (2025), doi:10.1038/s41586-025-09422-z；Ollama `deepseek-r1:8b` 的部署标签引用继续单独保留。

## 📄 LaTeX 构建状态（tex = 投稿实体）

**文件**：`manuscript_eacl2027.tex` + `appendices_eacl2027.tex` + `custom.bib` + `figures/`。**当前标题**：*Joint Training with Machine-Generated Text Detection Can Distort Sarcasm Detection: Diagnosing Class-Conditional Negative Transfer*。**编译**：`xelatex → bibtex → xelatex → xelatex`（中文需 XeLaTeX）。07-29 对摘要、引言、贡献、结果与结论作去重复防御化和术语统一，并进一步重组全文中突兀短句、模糊指代与承担多重功能的分号句；随后将摘要中的评估轴和方法建议统一为 `source $\times$ sarcasm`，避免引入未定义且容易与本文四象限术语竞争的 `source $\times$ class`；同时统一 `predicted human-authorship probability` 与 `LLM-judge ratings`，并将 §5.2 标题改为训练流程层面的 *Joint versus Single-Task Procedures*。同日按“惯用/有引用术语可保留，本文首次出现的非惯用术语须定义或改写”的边界清理正文、附录与中英同步稿：保留 PPO、DIP、RCNN、AUC、LEACE、INLP、QLoRA、BH-FDR 等领域名称；`class-conditional negative transfer` 在摘要首次出现时绑定 67.0 pp 现象，并在引言严格定义为集中于一个来源 $\times$ 讽刺子群、而非均匀分布于 DualTest 的负迁移；`task-branch interaction projection` 在 §4.1 以残差投影公式定义为部件，据此定义完整的 `interaction-projection configuration`；`srcORTH-PC1` 在正文 §5.3.2 首次以具体构造定义，附录 E.2 保留自足说明。训练协议另明确区分表征配置（plain / interaction-projection / RCNN）、训练流程（joint / sarcasm-only / MGTD-only）与任务头数量，表 2 以 joint 与对应 single-task procedure 为比较单位。普通设置、证据或比较关系改为直接说明实验操作与证据范围，`split-sample evaluation` 等旧称改写为拟合半/评估半的实际操作；§2.4 和附录 D 的标题也改为直接说明“检测器输出用作评价/奖励信号”以及“检测器概率与 LLM 评审评分的分歧”，不再把比较关系包装成命名现象。为解决 Times New Roman 环境中 Conclusion 漂至 p.9 的问题，tex 引言由约 601 词压至 449 词、相关工作由约 628 词压至 425 词：保留八段引言、四小节相关工作及三句 Contributions，删除重复的“本文做法/结果”叙述和引言/相关工作中的非必要章节指针。复审后补回“不重训检测器”、任务标签本身不冲突、`to the best of our knowledge` 及 M4/RAID 和共享表征/附加数据的准确引用边界。随后复核全文因果与推论连接：分别建立讽刺概率—讽刺评分和人类作者概率—类人度评分的动机；将 AUC 仅用于支持排序保留、由分数分布支持整体上移；把来源相关性证据与 srcORTH-PC1 敏感性作为并列证据；并收紧评审间一致性、参考文本比较和 Ethics 的依据。修改未改变实验数字、证据顺序或声明强度。附录 A.4/D 已补 Hanyu Pinyin 转写，`deepseek2025r1` 已更新为 Nature 正式版。最新沙盒 Times New Roman XeLaTeX 构建为 **21 页**：正文 §1–Conclusion 完整位于 p.1–8；Limitations 同在 p.8 开始；Ethical considerations 与 References 位于 p.9 起；附录从 p.12 起；`tab:lang-quadrant` 位于 p.19，附录 E 从 p.20 起。Mac 本地分页仍需终检。

**⚠️ 主字体修复（07-15，勿删；合规依据 07-15 已逐项核证）**：ACLPUB formatting.html §Fonts 原文："All text (except non-Latin scripts and mathematical formulas) should be set in **Times Roman**. If Times Roman is unavailable, you may use **Times New Roman** or **Computer Modern Roman**."——Times New Roman 为明文允许字体；官方仓库自带的 XeLaTeX/LuaLaTeX 模板 `acl_lualatex.tex` 本身就在 preamble 用 `\babelfont{rm}{TeXGyreTermesX}` 设置拉丁主字体（"similar to Times"），故在文档 preamble 设主字体=官方做法，非"修改样式文件"（acl.sty/acl_natbib.bst 未动）；CJK（非拉丁文字）与数学公式明文豁免于 Times 要求。xeCJK/fontspec 会把拉丁主字体静默重置为 Latin Modern，覆盖 `\usepackage{times}`——旧 PDF 因此主字体错误（aclpubcheck 报 Wrong font）。已在 preamble 的 xeCJK 块后加 `\IfFontExistsTF{Times New Roman}{...}{\setmainfont{TeX Gyre Termes}}`：Mac 上取 Times New Roman（PDF 字体名 TimesNewRomanPSMT，aclpubcheck 接受），无该字体环境回退 TeX Gyre Termes。**07-15 终检结果**：pdffonts 全嵌入 ✅；PDF 元数据无作者名 ✅；匿名化搜索（our previous/我们前期/Acknowledg/作者名）仅命中 References 第三人称自引 ✅；aclpubcheck 除 review 模式行号导致的 spurious margin 报错外，唯一真实错误即上述字体（已修，Mac 重编译后应消失）；无 undefined citations、无 overfull。

**格式合规（07-29 沙盒已过，待 Mac 终检）**：正文 §1–Conclusion 位于 p.1–8，Conclusion 完整止于 p.8；Limitations 同页开始。PDF 为 A4（595.28 × 841.89 pt），共 21 页；`[review]` 模式、行号/页码、双栏、匿名化、章节顺序和双栏附录均正常。**tex 摘要 196 词**（按 `detex | wc -w` 统计）。当前构建无最终未定义引用、重复标签、overfull 或 BibTeX 警告；仅有 3 条 LaTeX 自动将 `[h]` 浮动体调整为 `[ht]` 的非错误提示。

**07-29 当前覆盖说明（优先于下述 07-15 状态记录）**：当前 Contributions 为一段三句，按“资源与核心效应—稳健性与机制—探索性应用风险”组织；§5.2 标题为 *Joint versus Single-Task Procedures*；§6 标题为 *Exploratory Comparison of Detector Scores with LLM-Judge Ratings*；Conclusion 为一段且不含旧版 SFT 语言不对称句；正文表为 Table 1（backbone）、Table 2（joint vs. single）、Table 3（source $\times$ sarcasm），Spearman 与语言分层表位于附录。下述 07-15 内容仅记录当时压缩与移回过程，凡与本覆盖说明或当前 tex 冲突者均不得用于反向覆盖 tex。

**文档权威口径与 tex 相对 md 的压缩**：tex 是投稿文本与数值口径的最终权威源；md 是内容较全的同步长版。出现数字冲突时，以可复算的附录表、审计输出及 tex 为准，禁止用 md 反向覆盖 tex。贡献 4 条→三句一段（单一 softened "first"）；§2.4 收一段；§5.3.2 机制压两段 + **表 4 删**（数字全在附录 E）；~~主干对比表→附录 C.1；四象限图→附录 C.7~~（**07-15 用户决策撤销此两项**：正文有余量，主干表移回 §5.1、四象限图移回 §5.3.3，附录 C.1 只剩宏平均 F1 补充、C.7 只剩表格与配对检验）；§6→短节 "Detector–Judge Misalignment"（Spearman/逐评审/语言/案例全下沉附录）；结论一段；Limitations 改"承认而非再辩"；"five independent runs"→"five fold-based runs"。检索脚注、reward/RL future work、附录 E.2 指针已按 07-14 决策补齐。07-15 结论段补回 SFT 语言不对称一句（"The generation analysis additionally reveals a language asymmetry after SFT; …"），与 md 一致，勿当压缩差异回删；同日决策：结论保持一段，不重申 DualTest/双头贡献（避免第二处 novelty claim），future work 维持在 Limitations。**tex/en md 表号（07-15 移回后）：主干对比=表 1、双头vs单头=表 2、四象限=表 3、Spearman=表 4；架构=图 1、四象限图=图 2（正文 §5.3.3）**（zh md 表号仍自成体系）。**07-15 移回事故与修复**：首次移回时提取脚本误吞附录 A.2–C.1 整段入正文（编译件曾出现附录变正文第 6/7 章、18 页超标），已完整回退并重做——本次仅外科手术式移动两个浮动体本身，已静态验证：正文恰 7 个 \section、附录 A–E 结构与段序完整、table/figure 环境配平、tab:backbone 与 fig:quadrant 仅存于正文。**07-15 §4.1/§5.1 充实与代号清除（三稿同步）**：ⓐ §5.1 补一句三类编码器的预训练特征（mBERT=多语言维基 BERT-base / XLM-R-base=百语 CommonCrawl / DeBERTa-v3=解耦注意力+ELECTRA 式、英语为主）；ⓑ 附录 B.3/B.4 的两个中间层变体结构讲解（含维度、交互投影公式与**注意力权重恒为 1 的退化说明**）整体上移并入 §4.1——退化说明就是"为何不叫 cross-attention"的答案，正文现以否定形式出现一次 "rather than as a form of cross-attention"，属刻意，勿删；ⓒ **B4/B5 实验代号全文清除**（表 1 行名、附录 B.1 学习率表行名、附录段题），只用正式名 RCNN / task-branch interaction projection；原附录 B.5(SFT) 改号 B.3，已验证无悬空引用、无代号残留；ⓓ §5.1 重排为"先设置后结果"两段（设置段=比较范围+编码器特征+F1 口径，结果段=表 1 数字与结论；三稿同步，zh/en 的表格移至两段之间）——已核查其余实验小节（§5.2/§5.3.1/§5.3.3/§6）开头本就是先协议后结果，无需动；ⓔ preamble 新增 \Tabref/\Figref 宏并全量替换 Table~\ref/Figure~\ref，Table/Figure 字样纳入链接热区（与 \Sref/\Appref 同机制）；ⓕ 贡献描述位去防御化（07-15 用户指示，仅 tex——三处均为 tex 压缩时引入的对 zh 正面措辞的偏离，本次改回即恢复忠实）：结论 "but do not establish source identity as its causal mechanism"→"characterize … and delimit the remaining step to causal attribution"；§1 删 "and do not isolate source identity as its cause" 从句（边界由紧随的 hypothesis 定位句承担，zh/en 本无此句）；摘要 "rather than a causal dependence on source"→"with the source concept's specific causal role remaining a hypothesis"（对齐 zh"保留为待检验的假说"）。**现行口径**：§5.3.2 与结论均按“证据支持的机制范围 + 下一步因果目标”正向收束；Limitations 保留必要事实边界，但不得把旧的自我否定句重新引入贡献声明。**待办：Mac 编译核实正文（§1–Conclusion 含新表新图与 §4.1 扩写）仍在 8 页内**——表+图净增约 0.4 页，结论原止于第 8 页约 1/4 处，应有余量但须以编译 PDF 为准；若超页，优先回退四象限图（en md 同步回退 Figure 2 与表号）。

**07-20 排版覆盖（替代上述 07-15 当时状态）**：主干对比表与四象限表保留在正文；为确保 Conclusion 止于 p.8，跨生成器四象限图已由正文 §5.3.3 移至附录 C.7。md 保留内联图作为完整内容源，tex 为投稿排版实体。07-15 冻结的字面句 “rather than as a form of cross-attention”同时由 07-20 的残差投影公式化定义取代；保留的技术约束是单向量时无 query–key 交互、不称 cross-attention。

**录用后**：`[review]`→`[final]`；填真实作者/单位；References 前加不编号 Acknowledgments；正文 +1 页（9 页）。Ethics 节标题 "Ethical considerations"（匹配 ARR）。

## 关键日期（均为 AoE, UTC-12）

| 事项 | 日期 |
|------|------|
| **ARR 投稿截止** | **2026-08-03** |
| 全体作者审稿注册截止（强制） | 2026-08-05 |
| 作者回应期 | 2026-09-14 – 09-19 |
| 审稿人—作者讨论 | 2026-09-20 – 09-24 |
| 元审查发布 | 2026-10-08 |
| **EACL 承诺截止** | **2026-10-11** |
| 录用通知 | 2026-11-12 |
| 定稿（camera-ready） | 2026-11-26 |
| 主会议 | 2027-03-09 – 03-14 |

## 硬性要求对照

| 要求 | 规定 | 状态 |
|------|------|------|
| 论文类型 | 长文正文 ≤ 8 页（录用后 +1）| ✅ 07-29 沙盒正文完整止于 p.8；待 Mac 终检 |
| 不计页内容 | Limitations、Ethics、References、附录 | ✅ 结构已排（Limitations 在结论后、References 前）|
| **Limitations 强制** | 缺失即拒稿；不得含新方法/分析/结果 | ✅ 仅复述正文边界 |
| 模板 | 官方 ACL 样式，禁改样式；附录双栏 | ✅ 用 acl.sty `[review]`、附录双栏；投稿前跑 aclpubcheck |
| **双向匿名** | 无作者名/致谢；自引第三人称；补充材料匿名 | ✅ 前序工作 Yang & Ikeda 2023/2024 第三人称；全稿无"我们前期"|
| 预印本 | 若勾 "no preprint" 则不得预印 | 投稿时选择 |
| Responsible NLP Checklist | 表单填写；不实可拒稿 | ✅ 草稿见下 |
| 双重投稿 | 禁止 | 确认无并行投稿 |
| 审稿义务 | 全体作者 08-05 前注册 | ⚠️ 待办 |
| 论文完整性 | 禁幻觉引用 | ✅ 07-28 全表审计：68/68 key 对应，30 条一手来源逐字段复核；07-29 DeepSeek-R1 更新正式版 |
| AI 写作辅助 | 允许但须披露 | ✅ Ethics 已披露 |

## Responsible NLP Checklist 答案草稿（逐条，2026-07-21 结合论文与实验落定；填 OpenReview 表单用，不进正文）

> 官方问题：aclrollingreview.org/responsibleNLPresearch。表单答 Yes 须指向论文章节；下列"位置"即 justification 依据。许可结果为 2026-07-21 逐源查证（见后"五源许可"表）。

**Section A（每篇必答）**
- **A1 描述局限？ → Yes。** 强制 Limitations 节，三类边界（数据/设置、机制解释深度、生成评测规模与代理性）。
- **A2 讨论潜在风险？ → Yes。** Ethics Statement + §6：MGTD 检测器分数误用为生成质量/奖励信号、类条件失效对特定子群的系统性偏差、高风险场景部署警示。

**Section B（使用/创建科学工件 → Yes）**
- **B1 引用创建者？ → Yes。** §3 + References（五源数据集 + DeBERTa-v3/mBERT/XLM-R/Qwen3/DeepSeek-R1/QLoRA 等）。
- **B2 讨论许可/使用条款？ → Yes。** Ethics Statement + 附录 A "Licensing and release scope"：iSarcasm/CSC 的源仓库声明 MIT；其余三源未识别到明确再分发许可，因此不再分发其人类原文。表单 justification 指向这两处；Yes 表示论文已如实讨论许可与发布边界，并不表示所有来源都有开放许可证。
- **B3 使用与预期用途一致？ → Yes。** 五源均为讽刺检测研究公开发布，本文用于讽刺/MGTD 学术研究（§3、附录 A）。
- **B4 检查 PII/冒犯内容步骤？ → No（有解释，不构成不合规）。** Ethics Statement 如实说明：未采集新用户数据，但未在源语料发布时已有的处理之外另行开展 PII 或冒犯性内容审计。表单 justification 直接引用该说明；除非之后实际完成并记录额外审计，不改填 Yes。
- **B5 提供文档？ → Yes。** §3 + 附录 A（来源明细、标注方式、生成模板/参数/后处理、四象限统计、命名映射）。
- **B6 报告统计与划分？ → Yes。** §3 + 附录 A：SarcTrain 10,000（5 折）、AuthTrain 19,995（9,999 人+9,996 AI，90/10 留出）、GenTrain 2,428、DualTest 4,664（四象限 823/1,549/779/1,513）；协议 §4.2。

**Section C（计算实验 → Yes）**
- **C1 参数量/算力/基础设施？ → No（部分满足并解释）。** 附录 B 已列各编码器参数量（178M/278M/184M/435M）、Qwen3-8B≈8B、单卡 RTX 3090 24GB、检测器实验约数十 GPU 小时及 SFT 约两小时；但未给出每个完整检测器变体的精确参数量，也缺少本地生成与表示干预的完整 GPU-hour 记账。表单如实说明现有报告范围；若取得更完整日志再改 Yes。
- **C2 实验设置/超参搜索/最佳超参？ → Yes。** §4.2 + 附录 B 报告学习率分组、2:1 采样、类别权重、早停、5 折协议、最终模型固定轮数和验证选择规则；类别权重与采样比为先验设定，未依据 DualTest 调参。表单不额外声称论文未记录的搜索过程。
- **C3 结果描述性统计？ → Yes。** 五次运行均值±std（主表）、Wilson 95% CI、McNemar、Wilcoxon+BH-FDR、Spearman（§5、§6、附录 C.7）。
- **C4 现有包的实现/模型/参数设置？ → Yes。** 附录 B（torch 2.9.0/transformers 4.57.1/peft 0.18.0；DeBERTa-v3-large、Qwen3-8B+QLoRA、deepseek-r1:8b/qwen3.5:9b via Ollama、三盲评 LLM）。

**Section D（本研究是否新招募人工标注者/人类被试 → No）**
- 本研究未新招募、指导或付费给人类标注者/被试：人类文本与标签来自既有公开研究数据集，AI 侧标签由指令构造，§6 的三名"评审"是 LLM API 自动调用。位置：§3、§6、Ethics Statement、附录 C.3。
- 若表单在选择 No 后仍显示子项：D1/D2/D4 对本研究为 N/A；D3 说明本文未重新取得同意、数据获取与原始参与者程序由源数据研究负责；D5 说明本文不报告新的参与者人口统计，语料的语言、体裁与标注来源见附录 A。不要笼统声称原始研究的同意、伦理审批或人口统计已由本文验证。

**Section E（AI 助手 → Yes）**
- **E1 说明 AI 助手使用？ → Yes。** Ethics Statement 如实披露：AI-assisted tools 用于 language editing、structural revision 与 formatting，但未生成实验输出或统计；研究设计、分析与结论由作者决定并负责。录用版按 ACL 披露政策将相应说明纳入不编号 Acknowledgments。

### 五源许可（2026-07-21 逐源查证）

| 源 | 进入的数据集 | 许可 | 再分发原文 |
|----|------------|------|-----------|
| iSarcasm（iSarcasmEval, iabufarha/iSarcasmEval）| 训练 + SFT + **DualTest 英文侧** | 源仓库声明 **MIT** | 按许可条件处理 |
| CSC（CoPsyN/CSC）| 训练 | 源仓库声明 **MIT** | 按许可条件处理 |
| ToSarcasm（HITSZ-HLT/ToSarcasm）| 训练 + SFT + **DualTest 中文侧** | 所访问版本未识别到明确再分发许可 | 不再分发人类原文；直接发布须另获许可 |
| News Headlines v2（Misra, Kaggle/GitHub）| 训练 | 源仓库未识别到 LICENSE；Kaggle 字段**投稿前最后核实一次** | 不再分发人类原文 |
| Open Chinese Internet Sarcasm Corpus（Zhu 2022）| 训练 | 原链接失效，未识别到明示再分发许可 | 不再分发人类原文 |

**DualTest 发布决策（2026-07-21 定）**：不整体发布人类原始文本（中文侧 972 条来自未识别到明确再分发许可的 ToSarcasm）。采用与 README `Data licensing note` 一致的保守方案——**仅发布具备发布条件的 AI 生成文本、双标签/元数据与重建脚本；人类侧不分发原文，脚本要求用户从 iSarcasm/ToSarcasm 原始来源自行取得数据**。模型采用 MIT 许可不能单独推出所有输出均可不受限制地发布；发布时仍需遵守模型条款及种子语料的适用权利。若日后直接发布 ToSarcasm 中文原文，先取得数据权利人的明确许可。

## 🔒 生效红线与口径速查（改论文前必读，勿回改）

**格式合规红线（ⓕ 2026-07-17，最高优先级）**：EACL/ARR/ACLPUB 的一切格式要求 = 红线，任何内容改动不得违反；**所有可能导致 desk reject 的要求同为红线**。执行标准：① 样式文件（acl.sty/acl_natbib.bst）零修改，且**不得在导言区覆盖样式文件的刻意设定**（如 `\flushbottom`——07-17 曾为消除 float 段间空白加 `\raggedbottom`，属灰色地带已回退；美观问题一律让位于合规）；② 版式/字体类改动必须有官方依据才可做（指南明文或官方模板自身做法——如 Times 主字体修复、XeLaTeX 下 preamble 设字体均有 acl_lualatex.tex/ACLPUB §Fonts 背书）；③ 硬指标清单：A4、边距、字号、正文 ≤8 页、Limitations 节必有且不含新内容、附录双栏、`[review]` 模式（行号+页码）、双向匿名（无致谢/自引第三人称/无可追踪链接）、Responsible Checklist 如实填写、无幻觉引用、附录不得塞正文内容；④ 拿不准是否合规的改动默认不做，先查 ACLPUB formatting.html / ARR CFP 原文再动手。

**正向贡献表达红线（2026-07-24）**：摘要、引言、结果主题句与结论必须以“确立了什么、缩小了什么范围、提供了什么后续目标”为落点；不得用连续的“未能 / 不能证明 / 没有识别 / 仅仅 / rather than”作为贡献声明的主要收束。必要的因果边界统一写成“证据支持 X，并将 Y 界定为下一步分析目标”。本红线不得削弱 AI 侧标签构造、数据划分与泄漏复核、模型选择、实验对照、Limitations 与 Ethics 中的事实披露，也不得把相关证据升级为未经支持的因果结论。

**术语可解释性红线（2026-07-29）**：① 领域惯用或已有明确引用的方法名/缩写可以保留，不为形式统一强制展开，如 PPO、DIP、RCNN、AUC、LEACE、INLP、QLoRA、BH-FDR；② 本文自定义名称仅在确实指代反复使用的稳定对象、配置或核心现象，且首次出现立即定义时保留，包括 SarcTrain、AuthTrain、GenTrain、DualTest、task-branch interaction projection、srcORTH-PC1 与 class-conditional negative transfer；③ 普通实验说明、证据解释或比较关系不得包装成仿佛已有公认定义的名词短语，必须直接说明“如何设置、观察到什么、证据支持到哪一步”；若为反复指代确实需要压缩名称或简称，首次出现必须先用普通语言给出简要定义，随后才可使用简称。现已禁用 `binary MGTD`、`source-correlated shortcut hypothesis`、`fixed-detector evaluation`、`test-side replacement set`、`source-conditioned score shift`、`source-conditioned error allocation`、`focal subgroup/FPR`、`detector--judge comparison/study`、`configuration-level evidence`、`mechanism hypothesis`、`split-sample evaluation` 与 `split-sample orthogonal-direction control`；对应内容改写为人类–AI 二分类设置、不重训检测器的跨生成器评估、具体 AI 测试子集、AI 文本讽刺分数上移、来源组间错误分布、明确子群名称、检测器概率与 LLM 评分比较、直接披露同时变化的配置因素、表征级诊断，以及实际执行的拟合半/评估半操作。

**标签口径（ⓔ 2026-07-11 + 07-14；07-20 更新）**：AI 侧 `is_sarcastic` = 生成指令的**构造定义**。核心任务字段、四象限与指标继续使用 `is_sarcastic × is_human`、source $×$ sarcasm、AI $×$ non-sarcastic 和**误判率/FPR**，不全局改名为 sarcasm-reference / target-style 体系。`requested style`、`prompted condition`、`target` 仅用于局部说明 AI 标签来源，不替代任务与象限名称。**§3 与 Limitations 必须保留完整边界**：AI 侧讽刺标签记录 prompted condition，并非独立人工标注的 perceived sarcasm；摘要使用紧凑的正面事实表述“AI-generated text prompted to be non-sarcastic”，不必重复完整 caveat。AuthTrain 中作者来源标签与指令风格标签是分别构造、近似平衡的因素，**不假设实现后的文本特征或感知讽刺性独立**。单头 19.3% 反事实继续作为标签质疑的核心对照。2026-06 封存令维持（人类侧标签异构性不讨论）。

**因果口径**：失效"**由联合训练诱导**（induced by joint training）"不得降级为"相关/associated"（证据 = 同文本单头反事实 + McNemar）。

**机制证据口径（07-29 术语清理覆盖 07-24 旧称）**：不再把机制解释命名为 `source-correlated shortcut hypothesis`。直接陈述证据链：来源相关特征与受影响的讽刺决策存在 logit 相关和方向对齐；移除一个从数据中得到的高方差方向会改变来源组间的错误分布；该方向编码什么、为何影响预测以及来源身份的特异因果作用仍是后续分析目标。LEACE 与 srcORTH-PC1 的首要实证含义是**改变来源组间错误分配，而非降低来源平均的非讽刺误判**（58.8%→59.1% 为 LEACE 对照）；不得将其称为修复或整体缓解。**禁** source causes failure / model relies on source / “证明捷径” / “假说失败”。srcORTH-PC1 仅与所估计的线性来源方向正交；该性质不等于独立于所有多维或非线性来源信息。方法引用挂靠：INLP=Ravfogel 2020、LEACE=Belrose 2023、选择性=Hewitt & Liang 2019、amnesic=Elazar 2021。

**术语（英译/全文）**：plain dual-head / single-head（禁 naive）；task-branch **interaction projection**（禁 cross-attention / cross-projection）。其首次正文定义以残差投影公式 $h_s'=h_s+P_{a\rightarrow s}(h_a)$ 及对称式为准，并在同处定义采用该部件的完整模型简称为 **interaction-projection configuration**；单向量设置中原 attention 权重恒为 1，不存在 query–key 交互。07-15 的原句 “rather than as a form of cross-attention”已由 07-20 的公式化定义取代，冻结的是计算语义而非该字面句子。正式任务名统一为 **machine-generated text detection (MGTD)**，中文为“机器生成文本检测”，任务头/指标写 MGTD head / MGTD F1；具体样本写 AI-generated text；`authorship verification` 仅用于 Bevendorff 等范式讨论；禁 AI-authorship detection、authenticity/veracity；“类人度/human-likeness”仅 §6 盲评维度；核心现象名为 **class-conditional negative transfer**，摘要以 67.0 pp 集中误报率增幅作指示性说明，引言首次严格定义为集中于一个来源 $\times$ 讽刺子群而非均匀分布于 DualTest 的负迁移；`class-conditional failure` 仅作结果概括（禁 OOD failure；§2.3 引 Mueller"分布外"是文献描述、可留）。跨生成器部分直接写为**同一组检测器不经重训，在 outputs from another generator / Qwen3.5-generated outputs 上评估**（禁 `fixed-detector evaluation`、`test-side replacement set` 与 cross-family）。正式模型名与部署标签分开：DeepSeek-R1-0528-Qwen3-8B（Ollama `deepseek-r1:8b`，Qwen 基座）与 Qwen3.5-9B（Ollama `qwen3.5:9b`）；SFT 基座 Qwen3-8B 是另一模型。

**数据集名**（投稿版；发布代码/数据对齐，长版/JSON 用内部名）：SarcTrain=D_critic_sarcasm(10,000 全人类)/AuthTrain=D_critic_auth(19,995 = 9,999 人+9,996 AI)/GenTrain=D_actor_sft(2,428)/DualTest=D_test_dual(4,664)。"Master Metadata"→"标准化合并后的源数据总集"。

**数据划分复核（07-21，本地代码与 CSV 实测；07-22 口径复核）**：① SarcTrain 中文讽刺桶的过采样输入为 2,396 条记录、对应 2,394 个唯一文本，新增 104 条副本后达 2,500；过采样先于 shuffle 与五折分配，104 条副本中 86 条与原件跨折。按论文重复率所用的原始 text 且包含过采样副本口径，共有 116 组重复文本（241 行），其中 98 组跨折；排除新增副本并规范化空白与大小写后，自然重复为 19 组（46 行），其中 16 组跨折。两种口径不得混称或相加。该现象只作用于训练折—验证折的早停选轮，不进入独立 DualTest。② GenTrain 候选 5,257（英 2,972/中 2,285），种子 42 抽取 40%=2,103（英 1,214/中 889），再加 325 条中文过采样记录达 2,428；语言平衡先于 seed-42 的 85/15 SFT 训练—验证划分，故过采样副本可能跨两部分。独立的 300-prompt 生成评测集不参与该划分。

**训练—测试泄漏终核（07-24）**：修正后的记录级匹配复核确认，SarcTrain、AuthTrain 与 DualTest 的人类侧、AI 侧之间均无共享记录；最终结论为**不存在训练—测试泄漏**。早期纯文本匹配得到的“2 条 SarcTrain + 5 条 AuthTrain / 3 个唯一 DualTest 文本 / 删除后变化小于 0.1 pp”来自匹配程序问题，已从 tex、附录、英中 md、rebuttal 与 Limitations 全部撤销，禁止恢复。SarcTrain 内部跨训练折—验证折的重复记录是另一问题，只影响早停与检查点选择，不得与 DualTest 泄漏混称。

**优化与案例复核（07-21）**：检测器使用 AdamW，未显式设置 betas/epsilon（PyTorch 默认 0.9/0.999、1e-8）；LambdaLR 仅前 200 步线性 warmup，之后恒定。SFT 验证比例以实际脚本硬编码的 15% 为准，config 中 0.1 为未读取死配置。附录 D 案例为人工检查配对分数后选取，没有 disagreement 公式、排序脚本或 top-k 实现；禁止事后补写公式。Ollama digest 不进论文：本地仅能确认 qwen3.5:9b digest 6488c96fa5fa，deepseek-r1:8b 已不在本地、历史 digest 无法复核。

**§6 定位（07-21 更新）**：探索性下游比较，检验 `P(human)` 能否代理所选 LLM 评审的 perceived human-likeness，以及讽刺概率是否跟踪 LLM 评分；两者不是同一构念。统一写 detector probabilities / LLM-judge ratings，不能把 `P(human)` 直接称为 human-likeness，也不能外推为人类感知质量。本文未实施奖励优化；结论仅为检测器分数用作质量代理或奖励前需要外部验证。中英文提示对应不同任务，跨语言差异不是纯语言效应。

**写作三原则（全文）**：① 正面措辞（确立了什么 + 还差什么工具；不堆叠"未能/不主张"）；② 零伏笔（§3/§4 只陈述执行、不预演失效；协议差异按流程平铺、不用"理由"框架）；③ 去辩护化（拒绝辩护姿态、总分结构、只给正面理由）。章开头不念目录、不预报结果数字。

**数字口径**（07-11 已逐项对账；LEACE 自检 07-21 更正）：§3 人类侧 **1,400 英/972 中**（非 1,423/949）；§5.1 观测标准差减小 **约 34–47%**（非“方差”）；参考评审均分 **3.77**（非 3.78）；白化对齐 **|cos| 0.41（probe 方向）/0.49（MGTD 头方向）分开报**；probe **0.986 / 置换 0.512**（单头 0.974）。LEACE 定义性质的正确自检为：拟合半样本类均值差相对残余 **7×10⁻⁶**、同半样本线性 probe AUC **0.514**；旧的 **0.042 禁止恢复**。干预后重训 probe 的评估半 AUC **0.589**、84.8%、86.3% 等结果不变。§5.3 四象限用各自 final 模型点估计。

**引用口径**：P1/P2 = Yang & Ikeda 2023(IIAI-AAI,1–6)/2024(IJSKM 8(2)) 第三人称；ViSP = 期刊版 *Neurocomputing*（勿写 arXiv 2025）；CSC = Jang & Frassinelli 2024 + Jang, Braun & Frassinelli 2023（双引）；ToSarcasm = Liang et al. 2022 CCL 557–568；Open Chinese = Zhu 2022（勿给失效 GitHub）。⚠️ Jang 2023（intended vs perceived）仅作数据集引用，勿重开标签口径讨论（2026-06 封存）。

**不采纳（勿被审稿/修订稿推回）**：标签改名 / 因果降"相关" / 人工标注（无志愿者）/ 用 3-LLM 核验 AINS 标签（留 rebuttal）/ MLM 继续预训练 / 预算匹配 / 采样比敏感性 / 梯度冲突测量 / B-W2 多语言编码器 probe 复现 / W4 数据增广基线 / 基于伪匹配的泄漏删样、重训或敏感性分析 / 评价者修订稿整体弱化姿态。

**答辩弹药（专家四问 + 标签）**：① 为何讨论来源相关结构→logit 相关、方向对齐与定向干预共同把它和受影响决策联系起来，但正文不再把这组证据包装成命名假说；② srcORTH 有效否定 LEACE？→正是本文发现、严谨性优势；③ 与主任务关系→AI×非讽刺是部署真实输入，高误判 = 鲁棒性问题非噪声；④ 为何不做因果中介→目标是识别刻画、留 future work。标签攻击→单头 19.3% = 标签噪声上界论证 + 构造定义口径。

## 📖 已解决历史归档（决策日志，倒序；细节可查 git / 记忆 `project_paper3_writing_dir.md`）

- **07-29（§5.3.2 正文自足性补强）**：正文明确表征诊断使用产生 86.3% 误报率的单个 final plain joint model，并说明 probe 对照、普通余弦与协方差归一化对齐、held-out half 的基线、LEACE 的冻结表征擦除方式及擦除后重训来源 probe 的 AUC 口径；同时补入来源方向/INLP 的阴性结果，并将 srcORTH-PC1 限定为语义未确定的行为相关方向而非已识别来源特征。跨生成器段补 Qwen3.5 的 91.3%/7.9% 组成率并改称 observed gap，避免暗示不同保留集合上的严格配对增长。未移入附录表；全量构建仍为 21 页，Conclusion 完整止于 p.8。

- **07-29（老师意见落地、术语/连贯性/格式终检与清单同步）**：① 标题删除非核心的 bilingual 尾部，现为 *Joint Training with Machine-Generated Text Detection Can Distort Sarcasm Detection: Diagnosing Class-Conditional Negative Transfer*；② 摘要、引言、结果与结论减少重复防御性限定，AI 标签在高层叙事中用正面构造定义，完整人工感知边界继续冻结在 §3 与 Limitations；③ AI 侧 AUC 统一明确为 0.82–0.88，固定表达统一为 class-conditional negative transfer / prompt-specified style labels / predicted human-authorship probability / human-likeness rating / LLM-judge ratings，并将摘要的评估轴和方法建议统一为 `source $\times$ sarcasm`，避免引入未定义的 `source $\times$ class` 记法；进一步按术语可解释性边界清理 `binary MGTD`、`source-correlated shortcut hypothesis`、`fixed-detector evaluation`、`test-side replacement set`、`source-conditioned score shift`、`source-conditioned error allocation`、`configuration-level evidence`、`mechanism hypothesis`、`split-sample evaluation`、`focal subgroup/FPR` 与 `detector--judge comparison/study` 等非惯用自造短语，改为直接说明实际设置、具体子群、观测变化及证据范围；PPO、DIP、RCNN、AUC、LEACE、INLP、QLoRA、BH-FDR 等已有惯例或引用的名称不作机械展开；`class-conditional negative transfer` 由摘要的 67.0 pp 现象说明和引言的来源 $\times$ 讽刺子群操作性定义共同固定，`task-branch interaction projection` 明确为部件、`interaction-projection configuration` 为完整配置，`srcORTH-PC1` 已在正文首次出现处定义并与附录 E.2 对应；训练协议与表 2 明确分离 configuration、joint/single-task procedure 和任务头数量；④ 全文复扫并重组突兀短句、模糊 `This/These/their` 指代、过密分号句与段末功能跳转，尤其将共享编码器动机整理为“MGTD强化来源表征→来源线索进入讽刺头→可能产生负迁移”的单一因果链，并将跨生成器结果移回稳健性叙事；§5.2 同步改名为 *Joint versus Single-Task Procedures*，明确比较单位是完整训练流程而非单纯 head 数量；同日继续复核全文因果与推论连接，分别建立两类检测器概率比较的动机，拆开 AUC/分数分布的证据作用，将来源相关性与 srcORTH-PC1 干预写为并列证据，并收紧附录统计概括及 Ethics 的理由；未改变实验数字、证据顺序或声明强度；⑤ 为解决 Times New Roman 环境中 Conclusion 漂至 p.9 的问题，tex 引言由约 601 词压至 449 词、相关工作由约 628 词压至 425 词（净减约 355 词）；保留八段引言、四小节相关工作和三句 Contributions，删除三处重复的“本文做法/结果”叙述层及其中全部非必要章节指针，技术章节中的复现与证据定位指针保留；复审后补回“不重训检测器”、任务标签本身不冲突、`to the best of our knowledge` 及逐项引用边界，避免压缩影响审稿人对实验设计和证据归属的理解；⑥ 附录 A.4 与 D 的中文提示和案例补 Hanyu Pinyin，满足 ACLPUB 对非拉丁文字转写的要求；⑦ `deepseek2025r1` 更新为 Nature 645:633–638 (2025), doi:10.1038/s41586-025-09422-z，Ollama 模型卡引用继续承担部署 tag 映射；⑧ 最新 Times New Roman XeLaTeX 全量构建 21 页，摘要按 `detex | wc -w` 计 196 词，正文/Conclusion 完整止于 p.8，Limitations p.8，Ethical considerations 与 References p.9，附录 p.12，`tab:lang-quadrant` p.19、附录 E p.20；A4 595.28×841.89 pt、匿名 review 模式正常，无 overfull、未定义引用或 BibTeX 警告，仅有 2 条 `[h]` 自动调整为 `[ht]` 的非错误提示。Mac 字体与分页终检仍为投稿前待办。
- **07-27（图表可读性与粗体语义统一，三稿同步）**：ⓐ 图 1 脚本改两处文字并重跑——`authorship head`→`MGTD head`、`intermediate module (RCNN, main model)`→`intermediate module (RCNN)`（这两处是全投稿包最后的旧口径残留；`main model` 在 tex/md 中已零命中）；图 2 legend `(main)`→`(original)`。几何、字号、配色、尺寸、字体设置全部未动，重跑仅像素级抗锯齿差异（已对同环境基线做逐像素比对：图 2 差异仅限 legend 区域）。ⓑ 粗体语义：Table 3 caption 增 "Bold marks the focal AI non-sarcastic subgroup."；Table 10 删 0.895/0.730 的粗体；Table 17 caption 说明粗体=两项诊断性干预而非性能最优，并补 "AUC is the source-probe AUC after erasure."。ⓒ Table 12 由 `table`+`\resizebox{\columnwidth}` 改为跨栏 `table*`（去缩放、`tabcolsep` 6pt），列名 `Dual/Single pos.`→`Dual/Single FP` 并在 caption 定义。ⓓ 可读性：Table 4 的 ToSarcasm 语言列按用户确认改为纯 `Chinese`（原 "Chinese 3,892 / English 6" 三稿全删）；Table 6 行名缩为 `Backbone/plain dual`、`Inter.-proj. dual`，学习率改数学式并同步 B.1/B.3 正文；Table 9 列序改 `Base, SFT, adj. p`（数值随列同调）；Table 10 `P(sarc.)`→`P(sarcastic)`；Table 13 `Human·zh/en`→`Human×zh/en`；Tables 14–16 caption 补 `L = language; S = model condition; 斜线列 = 检测器概率 / LLM 均分`。ⓔ 中英 md 已同步全部适用项（md 无 Table 9/12 对应表；案例表本就分列、无需 L/S 说明）。**未改任何数值、未动样式文件、无新增内容主张**；唯一影响正文页数的是 Table 3 caption 一句，待 Mac 编译核实。
- **07-24（训练—测试泄漏终核）**：纠正早期纯文本匹配程序产生的伪匹配；记录级复核确认 SarcTrain、AuthTrain 与 DualTest 两侧均无共享记录。三稿、正式附录、Limitations 与 rebuttal 已统一删除“7 条/3 个唯一文本/小于 0.1 pp”及训练侧影响表述，冻结为“无训练—测试泄漏”。训练/验证内部重复的披露继续保留。
- **07-21（本地实现四组终核）**：① digest 整体不写，避免不可复核且会随 Ollama 更新变化的实现字符串；正式模型名、Ollama 标签、生成参数、模板和后处理继续保留。② 澄清 SarcTrain 中文讽刺桶 2,396 records / 2,394 unique texts / 104 oversampled records，跨折副本仅影响早停验证，不污染 DualTest 数字，详细统计入红线备查而不在论文扩展。③ GenTrain 改为精确流程：5,257 候选→seed 42 抽取 2,103→中文加 325→2,428；附录补语言平衡后 85/15 验证划分。④ B.2 补 AdamW 默认 betas/epsilon 与 warmup-only LambdaLR；B.3 明确真实验证比例 15%。⑤ 附录 D 改为人工选取高分歧案例，明确无公式或 top-k，未事后虚构选择算法。
- **07-21（§7–Ethics、References、附录 A–E 旧建议复核；泄漏项由 07-24 终核覆盖）**：① Conclusion 不再使用泛化的 matched/mitigation/standalone 信号表述，保留同架构同讽刺数据对照、方向性跨生成器复现和配置级差异。② §5.3.2 与附录 E 将 LEACE/srcORTH 解释改为来源组间错误重分配；`cross-fitted` 改为实际执行的 `split-sample evaluation`，保留 0.589、0.986、0.512、84.8%、86.3% 等数字。③ 当时曾在 Limitations 增补后经确认属于匹配程序伪匹配的“7 条 AI 侧重叠”；该项已由 07-24 终核撤销，其余 AI 指令遵循、80 条未配对、类别权重、训练预算、优化不匹配及评审构念边界继续保留。④ Ethics 改 open-weight，披露 API 实际传输范围，加入 individual-text authorship/misconduct 滥用风险。⑤ 修正 C.6 将 60:40 错写为 SFT 训练比例的事实错误（GenTrain 实为中英各 1,214）；收紧 C.1/C.2/C.5 与案例推断。⑥ 模型正式名与 Ollama 标签分离，Qwen3.5 引用采用官方推荐条目，Gemini 2.5 报告作者改为 Comanici et al.；News Headlines 双引与 mode+1 真实训练规则保留。⑦ 三稿同步后 XeLaTeX 全量构建 21 页，Conclusion p.8 后段、附录 p.13，无 overfull/最终未定义引用。
- **07-21（§5–§6 与附录事实复核及平衡压缩）**：① DualTest 同时用于描述性架构比较与子群诊断，正文明确其不参与训练、验证、阈值或 epoch 选择；不再把 RCNN 写成由 DualTest“selected”的 main model。② 双–单头对照披露真实调度差异：单头每轮遍历完整讽刺训练折，双头按 2:1 混合调度抽取讽刺批次；结论指向完整联合训练流程。③ Table 2 恢复五次运行均值±标准差；Wilson 区间限定为条件于固定模型的实例级不确定性。④ 核心现象补入 AI 侧 AUC 0.82–0.88、67.0 pp 的匹配子群差距与来源条件化分数上移；跨生成器保留差距方向及架构层级证据。⑤ §5.3.2 保留 probe—行为对齐—定向擦除—正交/随机控制的完整诊断链；§6 改为探索性 detector-probability/LLM-rating 比较，区分 human authorship 与 perceived human-likeness，并保留条件级反转。⑥ LEACE 错误自检数字 0.042 已从三稿清除，改为 7×10⁻⁶ / 0.514；GPT-5-nano 四条不可解析评分（英 1、中 3）与成对删除已说明。⑦ 合并 §5.1/§5.2/§5.3/§6 重复限定，将容量匹配、广泛生成器覆盖与 RL 等边界集中到 Limitations；再补回能强化贡献的精确效应量与解释，最终全量重编译为 20 页，Conclusion 位于 p.8 后段并接近页底，附录 p.12，无 overfull/未定义引用。
- **07-20（标签、架构与最终模型口径拍板）**：① 不推翻 07-11 的核心标签体系，四象限仍为 source $×$ sarcasm，表头与结果仍使用 AI $×$ non-sarcastic 和 FPR；requested/prompted 用语只说明 AI 标签来源。② 摘要不再重复完整人工标注 caveat，以 “prompted to be non-sarcastic”紧凑披露；完整边界冻结在 §3 与 Limitations。③ AuthTrain 只主张作者来源与指令风格是分别构造、近似平衡的因素，不主张生成文本特征或感知讽刺性统计独立。④ 交互投影以实际残差投影公式定义，取代 07-15 冻结的具体否定句，但保留“无 query–key 交互、不称 cross-attention”的技术口径。⑤ final 模型的固定 epoch 数明确由五次验证运行决定；附录保留“五个最佳 epoch 众数 + 1，上限 10”的完整规则，不经 DualTest 选择。⑥ Bevendorff 的 closed-set attribution / open-set verification 修饰词已按原文核实可保留；`AI × non-sarcastic subset` 是 §3 定义后的合法短式。中英 md 已同步标签独立性、配对覆盖率与交互投影公式。
- **07-15（动机与结论微调，三稿同步）**：§1 首段"部署侧双判断"后加一句生成侧动机（检测器分数充当讽刺生成评价/奖励已进入实践，cite ViSP + §2.4 指针；评价生成讽刺是否像人所写天然涉及两维度），消除"两任务强行组合"观感——reward 弧线由三处变四处（§1 动机提及 → §6 原理可充当 → ρ 证伪 → future work），口径仍是"实践存在/原理可能"，勿写成本文目标；tex 结论段补回 SFT 语言不对称一句（详见"tex 相对 md 的压缩"条目）；章节引用清理（三稿同步删 3 处冗余指针：GenTrain 段末重复的 §6、§4.2 公式段 z(x) 后的 §4.1、五折句的 §3；其余 § 引用经逐处分析均承担 claim 支撑/弧线衔接/协议透明职能，勿再批量增删）；§1 诊断句 "replicates with a second generator"→"persists when the AI test texts are regenerated by a second generator"（三稿同步；原句紧跟 induced by joint training 易误读为换生成器重新训练，实际只重生成测试集 AI 侧；摘要与 §5 的 replicated/cross-generator replication 术语不动）；术语统一排查（07-15，三稿同步；07-20 再统一）：ⓐ auxiliary source task→auxiliary authorship task，三稿现统一为 authorship task / 作者身份任务；ⓑ §2.3 结尾 induced by joint learning→joint training（冻结因果句式，zh 同改；摘要"联合学习行为"的领域泛指不动）；ⓒ intermediate 词族立分工规则——**module=具体部件（RCNN/交互投影），structure=可为空的中间槽位/配置（"three intermediate structures"含 plain 无模块，勿改成 modules），layer=机制/作用讨论（intermediate-layer buffering 及 screens out 句）**，按此规则改 3 处（§4.1 "two additional structures→variants"、§5.2 部件列举 layer→structure、§5.2 段末 same two intermediate structures→modules）；§1 的 "source identification"/"semantic judgment" 引号抽象角色名、§2.2 检索脚注引号内 "authorship detection"、classifier 两处角色陈述均为刻意，勿归一。§5.3.2 tex 补回安慰剂从句 "whereas ten random-direction placebos leave the rate unchanged"（07-13 压缩时断掉的逻辑链一环：无此句则"擦正交方向同样有效"可被反问"任意方向皆有效？"；md 两稿本有此环节未动）；§2.1 压缩 P1 句（三稿同步）：P1 折为 P2 句内从句"building on their earlier multilingual mBERT/BiLSTM study"（P1 形态类型学内容 P3 未使用，Y&I 占比由近半降至约 1/3；勿写回"P1 也是 RCNN"），并补 RCNN 原始出处 Lai et al. 2015（AAAI, mBERT-RCNN 括注 + custom.bib 新增 lai2015rcnn，doi/pages 按 OJS 页面填写，编译后随手核对渲染）；附录引用全部改为可点击链接（章级 Appendix~\ref{app:*}，小节级 \hyperref[app:c1/c7/a4/c3]{...} + 附录内 \phantomsection\label 四处，渲染文字不变；已验证两 tex 中所有字面 Appendix~X 均在链接内，无残留）；§2.3 删梯度冲突免责句（"we list gradient conflict as one possible cause and leave its direct measurement to future work"——limitation 式声明不属 related work，且与 tex Limitations 已有的 "direct gradient analysis" 重复；三稿同删，两 md 的 Limitations 补 "and direct gradient analysis"/"与直接的梯度分析" 保持内容对齐；§2 各小节末尾的定位句是规范写法，勿删）；News Headlines 数据集改按官方双引用（07-15 用户提供）：Misra & Arora 2023（AI Open，misra2023headlines 原条目已正确，key 保留）**+ 新增 Misra & Grover 2021《Sculpting Data for ML》**（custom.bib: misra2021sculpting，ISBN 9798585463570）；§3 引用点三稿改双引，两 md 参考文献表插入新 32 条、后续条目已重编号（55→56 条）；交叉引用链接区扩展（07-15）：preamble 新增 \Sref/\Appref 宏（\hyperref+\ref*，使 §/Appendix 字样整体可点而非仅编号），两 tex 全部 \S\ref、Appendix~\ref、Appendices 区间与 Section~\ref 已改写为宏/包装形式，渲染文字不变。
- **07-15**：9 条文献全部核毕、custom.bib 修 4 处（Compton venue+pages / Du 作者 / Li doi / Ye→TMLR 2026 Clever Hans Mirage）；发现并修复 xeCJK 覆盖 Times 主字体问题（详见构建状态）；沙盒跑通 pdffonts / aclpubcheck / 匿名化 / 元数据终检；PDF 重编译 19→18 页；删除杂项 .out 空文件与 aclpubcheck 输出。
- **07-14**：标签 caveat 同步进两 md（详见速查·标签口径）；`SUBMISSION_NOTES.md` 并入本文件并删除；补检索脚注 / reward future work / 附录 E.2 指针入 tex。
- **07-13**：英文稿 8 页压缩（摘要 200 词、§5.3.2 压三段删表 4、主干表→C.1、四象限图→C.7、表号顺延）；架构图 = 图 1（`figures/fig_architecture`），四象限图顺延；术语全同步 plain / interaction projection / authorship。
- **07-12（A/B 二轮，A 3/5 B 3.5/5）**：§6 全改"LLM 盲评/独立质量评价"（去"人类感知"）；贡献 5→4；跨生成器表删（与图同源）+ Spearman 改表号；§5.2 改逐次配对差值（讽刺 10/10 负、真实性 8/10 正）；C.3 补盲评协议 + 评审间 ρ；§2.2 检索降脚注；B.2 补软硬件。
- **07-12（A/B 一轮，A 2.5 B 3）**：B5 改名"任务分支交互投影"；AuthTrain 真相 9,999+9,996；分条件 ρ 重锚（真实性 −0.212 主由条件间驱动）；单头对照措辞精确化；图新增（Wilson CI）；统计升级列为 Strengths。
- **07-11（模拟 2.0/5→定位诊断论文争 Findings）**：五项拍板 ⓐ§6 压缩 ⓑ钉接句"相关但独立" ⓒ允许重打分升级统计（Wilson/bootstrap/McNemar CI）ⓔ标签构造定义 + 标题保留/术语不迁移（详见速查）；新产物 dump_dualtest_predictions.py / compute_ci_stats.py；附录去重；全文 50+ 数字对账（关键修正见速查·数字口径）。
- **07-09**：术语 OOD→类条件；跨家族全删；机制升级"修正假说"；§5.3.2 展开 + 补方法引用 + 起表 4；§5.4 / §5.3.4 / 文献关系小节删；Limitations 重组三类边界；§7 结论去重；全文去辩护化三原则通改；单头反事实术语落地；章开头去目录化；§5 导语重写（更正"5 折模型"误述）。
- **07-08**：§2 六节并四节；§4 方法章重写（4.1 任务架构含主模型 RCNN / 4.2 训练协议公式化 + 五次运行固定外部评测协议 + 最终模型交代 / 删 §4.3）；§3 构建透明度补强（claim-critical 双标签构造句）；数据集论文名（SarcTrain 等）；数据集引用补齐（CSC 双引/ToSarcasm/Open Chinese）；ViSP 更正期刊版；零伏笔纪律。
- **07-07**：正面措辞改写（assert findings, scope the gap）；ViSP 融入（前提检验/实证警示，不否定）；外部语气/去 AI 味修订合并。
- **07-04**：防御性修订（证据分层→后简化；srcORTH 提正文；§1 风险探针框架；§6 nuance 钉接句；Limitations scoping）。

## 与长版稿的关系

- `eacl2027/manuscript_eacl2027_zh.md` = 投稿版中文权威源（8 页预算 + 匿名 + ARR 结构）；`manuscript_eacl2027_en.md` = 英译；`manuscript_eacl2027.tex` = 投稿实体（更进一步压缩，见构建状态）。
- `paper3/manuscript_zh.md` = 长版权威稿（数字 single source = `docs/sft_eval_v1/paper_outline_detection_v1.md`），未同步本轮全部改动。
- 主要结构差异：长版 §6+§7 在投稿版合并为 §6；长版 §8.2 局限移出为不计页 Limitations；表合并（长版表 6+8→投稿四象限表）；新增 Ethics。数字全部一致、无新数字。
