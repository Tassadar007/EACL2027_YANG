# EACL 2027 投稿合规清单与待办

> 依据：https://2027.eacl.org/calls/papers/、https://aclrollingreview.org/cfp 与 https://acl-org.github.io/ACLPUB/formatting.html（2026-08-02 复核）。投稿路径：**ARR（OpenReview）→ EACL 2027 承诺（commitment）**。
> 本文件 = 投稿工作总账。结构：① 待办 ② 构建状态 ③ 日期/合规/Checklist ④ **生效红线速查（改论文前必读、勿回改）** ⑤ 已解决历史归档。

## 🔴 投稿前待办（TODO，按优先级）

1. **OpenReview/ARR 投稿表单**：**逐项填写指导见 `submission_form_guide.md`（2026-08-02 新建，含可直接粘贴的英文 justification、老师需做事项与官方 URL、提交当天操作顺序）**。Responsible NLP checklist（答案见 §Checklist）、preprint/匿名选择、AI 写作披露、作者名单与适用时的 resubmission 信息；ARR 提交后作者名单不可随意变更。
2. **全体作者 ARR 审稿注册**（2026-08-05 EoD AoE 前，强制）。
3. **最终 PDF 变更纪律**：当前项目 PDF 已包含 §5.4 的 Q1--Q3 结构、更新后的 Figure 3，以及 §6 对三组 detector--judge 结果的直接归纳。正文与 Conclusion 完整止于 p.8，Limitations、Ethical considerations 与 References 从 p.9 起，附录 A 从 p.13 起。PDF 使用 TimesNewRomanPSMT，23 个字体对象全部嵌入；无作者元数据、Overfull、未定义引用或 BibTeX 警告。当前 SHA-256：`0e6f38f952ff7d6a888a20b66661e3bad1bc3e57946f6ea2a1aeff70e32bbc5b`。此后若再改 tex、附录、图片或 bib，必须重新构建并更新本记录。
4. **数据许可（2026-08-02 当前口径，见 §Checklist "五源许可"表）**：iSarcasmEval 与 CSC 的源仓库声明 MIT；News Headlines v2 的 Kaggle 发布页声明 CC BY 4.0；对 ToSarcasm 与 Open Chinese Internet Sarcasm Corpus 的所访问版本未识别到明确再分发许可。本轮代码与数据均不上传，也不作录用后发布承诺；附录 A.1 仅说明可再分发范围与重建方式，不构成发布承诺。
   **② 附件决策（2026-08-02 定）：本轮代码与数据均不上传。** 数据侧正文从未承诺 release（A.1 只界定可再分发范围），故无冲突；代码为可选附件。**🔴 且不做代码或数据的发布承诺（作者 08-02 决定，已推翻早前"在 checklist 写录用后发布"的方案）**：checklist 与表单任何位置都不出现 "will be released upon acceptance" 之类句子；带 "If you are releasing…" 条件的小问一律答 N/A。正文已核查零发布承诺（`will release|will be released|upon acceptance|plan to release` 全零命中），附录 A.1 的"可再分发范围"属条件性说明、保持原样。不往正文加脚注（正文卡在 p.8）。匿名代码包已备妥留作内部备份：`anonymous_code/` + `anonymous_code.zip`（41 文件 / 148 KB）。若未来改变决定，可直接上传匿名 `.zip`/`.tgz`，或使用 Anonymous GitHub；不得使用会暴露账号的共享链接，并须复核 `.pyc`、绝对路径、项目标识与归档元数据。
5. **两阶段提交**：2026-08-03 23:59 AoE 前在 OpenReview 提交 ARR 2026-08 周期；取得完整 reviews/meta-review 后，于 2026-10-11 23:59 AoE 前另行提交 EACL 2027 commitment 表单。

（原"英译""ACL 模板排版""文献核实""中文示例拼音转写""aclpubcheck/pdffonts/匿名化/元数据终检"已完成，见当前构建状态与历史归档。）

### ✅ 文献核实结果（07-15 初核；07-28 全表审计；07-29 正式版本复核；08-02 当前闭环复核）

- **当前闭环复核（08-02）** ✅ 67 个被引用 key 与 67 个 bib 条目一一对应，无缺失、冗余或重复 key；其中主要条目已对 ACL Anthology、出版社、OpenReview、PMLR、CVF、DOI 或官方模型页面复核；BibTeX/LaTeX 无未定义引用。07-28 的 68 条统计属于当时版本，当前版本已移除不再使用的条目。

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

**文件**：`manuscript_eacl2027.tex` + `appendices_eacl2027.tex` + `custom.bib` + `figures/`。**当前标题**：*Joint Training with Machine-Generated Text Detection Can Distort Sarcasm Detection: Diagnosing Class-Conditional Negative Transfer*。**编译**：`xelatex → bibtex → xelatex → xelatex`（中文需 XeLaTeX）。当前正文保留：描述性主干比较与非模型选择边界；十次 fold-matched 比较及完整联合流程口径；67.0/14.8 pp 与 McNemar 检验；AI 内 AUC 0.82--0.88、概率中位数/IQR、人类--AI 和语言分层；Q1 来源可读性、Q2 分数/方向关联与 Q3 表征干预敏感性；拟合半/评估半协议、冻结讽刺头、评估半重新交叉验证 source probe；LEACE 0.589 与错误重新分配；INLP-3 residual PC1 后 gap 12.6 pp、source AUC 0.982 及非唯一因果边界；以及不重训检测器的 Qwen3.5 复现。当前全量构建为 **25 页**：Qwen3.5 结果位于正文 p.6，§6 与 Conclusion 位于 p.8，Limitations、Ethical considerations 与 References 从 p.9 起；附录 A/B/C 从 p.13/p.15/p.16 起，附录 D 与 E 均从 p.20 起。

**⚠️ 主字体修复（07-15，勿删；08-02 已再次验证）**：ACLPUB formatting.html §Fonts 原文："All text (except non-Latin scripts and mathematical formulas) should be set in **Times Roman**. If Times Roman is unavailable, you may use **Times New Roman** or **Computer Modern Roman**."——Times New Roman 为明文允许字体；官方仓库自带的 XeLaTeX/LuaLaTeX 模板 `acl_lualatex.tex` 本身就在 preamble 用 `\babelfont{rm}{TeXGyreTermesX}` 设置拉丁主字体（"similar to Times"），故在文档 preamble 设主字体=官方做法，非"修改样式文件"（acl.sty/acl_natbib.bst 未动）；CJK（非拉丁文字）与数学公式明文豁免于 Times 要求。xeCJK/fontspec 会把拉丁主字体重置为 Latin Modern，覆盖 `\usepackage{times}`；当前 preamble 在 xeCJK 后用 `\IfFontExistsTF{Times New Roman}{...}{\setmainfont{TeX Gyre Termes}}` 恢复 Times 系主字体。08-02 当前 Mac PDF 使用 TimesNewRomanPSMT，所有字体嵌入，元数据无作者名；沙盒回退 TeX Gyre Termes/Noto CJK 亦能生成相同正文八页结构。

**格式状态（2026-08-02 当前复核）**：PDF 为 A4（595.28 × 841.89 pt），共 25 页；正文 §1--Conclusion 完整位于 p.1--8，Conclusion 最后一行仍在 p.8，Limitations 从 p.9 开始。`[review]` 模式、行号/页码、双栏、匿名化、章节顺序和双栏附录均正常。**tex 与 OpenReview 纯文本摘要均为 192 词且逐字一致**。当前构建无未定义引用、重复标签、Overfull 或 BibTeX 警告；仅有 3 条 LaTeX 将 `[h]` 自动调整为 `[ht]` 的非错误提示。全部 23 个 PDF 字体对象已嵌入，PDF 元数据不含作者字段。

**08-02 当前覆盖说明（优先于全部历史状态记录）**：当前 Contributions 为一段三句，按“资源与核心效应—稳健性与机制—探索性应用风险”组织；§5.4 *Representation-Level Diagnosis* 以 Q1 来源可用性、Q2 与讽刺决策的关联、Q3 来源组间误报分配组织，Q3 下以粗体 `(a)/(b)/(c)` 区分三类干预；§6 标题为 *Exploratory Comparison of Detector Scores with LLM-Judge Ratings*；Conclusion 为一段。正文表为 Table 1（backbone）、Table 2（joint vs. single）、Table 3（source $\times$ sarcasm），完整统计与 representation protocol 位于附录。Figure 3、正文、附录和 Table 19 均使用 `INLP-3 residual PC1`，当前稿不得恢复旧称 `srcORTH-PC1`。下述日期日志只记录当时版本，凡与本覆盖说明或当前 tex 冲突者均不得用于反向覆盖 tex。

**文档权威口径**：tex 是投稿文本与数值口径的唯一权威源；中英文 md 是长版参考，不保证逐句同步。当前 representation analysis 明确区分 full-set probes/alignment 与 50/50 intervention split，不得恢复“一半表征”等歧义，也不得删除原讽刺头不重训、评估半内部重新交叉验证 probe、full-set unlabeled mean/covariance 的 transductive 披露、24.7% pair 跨半、LEACE 错误重新分配或 INLP-3 residual PC1 的非唯一因果边界。实现残差、置换标签、长度分层、各控制的完整数字及样本数由附录 E 承担。

**当前排版覆盖（2026-08-02）**：主干对比表与四象限表保留在正文；跨生成器四象限 Figure 2 位于附录 C.3（p.17），语言分层 Table 10 位于 p.18。Table 15 位于 p.20，Tables 16--17 位于 p.21；representation protocol Figure 3 位于 p.22，Table 18 位于 p.23，Table 19 位于 p.25。md 仅作长版参考，tex 是投稿排版与数值口径的唯一权威源。交互投影继续以实际残差投影公式定义；单向量时无 query--key 交互，不称 cross-attention。

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
| 论文类型 | 长文正文 ≤ 8 页（录用后 +1）| ✅ 08-02 Mac/沙盒双环境终检：正文与 Conclusion 完整止于 p.8 |
| 不计页内容 | Limitations、Ethics、References、附录 | ✅ 结构已排（Limitations 在结论后、References 前）|
| **Limitations 强制** | 缺失即拒稿；不得含新方法/分析/结果 | ✅ 仅复述正文边界 |
| 模板 | 官方 ACL 样式，禁改样式；附录双栏 | ✅ 用 acl.sty `[review]`、附录双栏；投稿前跑 aclpubcheck |
| **双向匿名** | 无作者名/致谢；自引第三人称；补充材料匿名 | ✅ 前序工作 Yang & Ikeda 2023/2024 第三人称；全稿无"我们前期"|
| 预印本 | 若勾 "no preprint" 则不得预印 | 投稿时选择 |
| Responsible NLP Checklist | 表单填写；不实可拒稿 | ✅ 草稿见下 |
| 双重投稿 | 禁止 | 确认无并行投稿 |
| 审稿义务 | 全体作者 08-05 前注册 | ⚠️ 待办 |
| 论文完整性 | 禁幻觉引用 | ✅ 08-02 当前版本 67/67 key 对应，无缺失、冗余或重复；DeepSeek-R1 使用 Nature 正式版 |
| AI 写作辅助 | 允许但须披露 | ✅ Ethical considerations 已披露 |

## Responsible NLP Checklist 答案草稿（2026-08-02 按当前文稿重新对齐；填 OpenReview 表单用，不进正文）

> 官方问题：aclrollingreview.org/responsibleNLPresearch。表单答 Yes 须指向论文章节；下列"位置"即 justification 依据。许可结果为 2026-07-21 逐源查证（见后"五源许可"表）。

**Section A（当前表单仅有 A1 Potential Risks）**
- **A1 潜在伦理、社会或环境风险？ → Yes。** Ethical considerations 讨论来源组系统性错误、把检测器输出当作作者身份或不当行为证据的风险，以及无人工复核和领域验证时用于高风险部署的风险；§6 另讨论把检测器概率复用为生成质量分数或奖励信号的风险。表单 elaboration 使用 `submission_form_guide.md` 中的英文段落。

**Section B（使用/创建科学工件 → Yes）**
- **B1 引用创建者？ → No（附说明）。** §3 与 References 已引用五源数据集、预训练模型和方法的创建者；但 PyTorch、transformers、NumPy、SciPy、scikit-learn、pandas、peft、datasets 等软件包仅在附录 B.3 报告名称与版本，未逐项提供创建者文献引用，因此不笼统回答 Yes。
- **B2 发布权利/许可？ → N/A。** 实际表单问题以 “If you are releasing…” 为条件；本轮不上传代码或数据。附录 A.1 仍记录已识别的语料许可与可再分发范围。
- **B3 若发布工件，PII 缓解？ → N/A。** 本轮不发布数据。Ethical considerations 另行如实说明未对既有语料执行额外 PII 审计。
- **B4 若发布工件，冒犯内容缓解？ → N/A。** 本轮不发布数据。Ethical considerations 另行如实说明未对既有语料执行额外冒犯内容审计。
- **B5 提供文档？ → Yes。** §3 + 附录 A（来源明细、标注方式、生成模板/参数/后处理、四象限统计、命名映射）。
- **B6 报告统计与划分？ → Yes。** §3 + 附录 A：SarcTrain 10,000（5 折）、AuthTrain 19,995（9,999 人+9,996 AI，90/10 留出）、GenTrain 2,428、DualTest 4,664（四象限 823/1,549/779/1,513）；协议 §4.2。

**Section C（计算实验 → Yes）**
- **C1 参数量/算力/基础设施？ → Yes（2026-08-02 已解决）。** 附录 B.3 已改写为：各编码器参数量（178M/278M/184M/435M）、Qwen3-8B≈8B、单卡 RTX 3090 24GB、**检测器训练与推理约 170 GPU 小时**，并明写该数字不含 Ollama 本地生成、SFT（约两小时）与在 CPU 上运行的 representation analyses。覆盖范围已显式化，故可填 Yes。170 h 由 wandb run 批次去重后得出（此前朴素求和 581 h 属重复计数）。
- **C2 实验设置/超参搜索/最佳超参？ → No（保守口径）。** §4.2 + 附录 B 已报告最终学习率、2:1 采样、类别权重、早停、5 折协议和最终模型 refitting 规则；附录 B.2 明确说明未进行系统性超参搜索，且 DualTest 未用于选择优化设置。但作者无法重建是否发生过有限的手工 pilot 调整，也无法报告所有此类尝试的结果，因此不声称完整记录了全部调参实验。表单使用 `submission_form_guide.md` 中的英文说明。
- **C3 结果描述性统计？ → Yes。** 五次运行均值±std（主表）、Wilson 95% CI、McNemar、Wilcoxon+Benjamini--Hochberg FDR、Spearman（§5、§6、附录 C.3--C.8）。
- **C4 现有包的实现/模型/参数设置？ → Yes。** 附录 B（torch 2.9.0/transformers 4.57.1/peft 0.18.0；DeBERTa-v3-large、Qwen3-8B+QLoRA、deepseek-r1:8b/qwen3.5:9b via Ollama、三盲评 LLM）。

**Section D（本研究是否新招募人工标注者/人类被试 → No）**
- 本研究未新招募、指导或付费给人类标注者/被试：人类文本与标签来自既有公开研究数据集，AI 侧标签由指令构造，§6 的三名"评审"是 LLM API 自动调用。位置：§3、§6、Ethical considerations、附录 C.4--C.5。
- 若表单在选择 No 后仍显示子项：D1/D2/D4 对本研究为 N/A；D3 说明本文未重新取得同意、数据获取与原始参与者程序由源数据研究负责；D5 说明本文不报告新的参与者人口统计，语料的语言、体裁与标注来源见附录 A。不要笼统声称原始研究的同意、伦理审批或人口统计已由本文验证。

**Section E（AI 助手 → Yes）**
- **E1 说明 AI 助手使用？ → Yes。** Ethical considerations 如实披露：AI-assisted tools 用于 language editing、structural revision 与 formatting，但未生成实验输出或统计；研究设计、分析与结论由作者决定并负责。录用版按 ACL 披露政策将相应说明纳入不编号 Acknowledgments。

### 五源许可（2026-07-21 逐源查证）

| 源 | 进入的数据集 | 许可 | 再分发原文 |
|----|------------|------|-----------|
| iSarcasm（iSarcasmEval, iabufarha/iSarcasmEval）| 训练 + SFT + **DualTest 英文侧** | 源仓库声明 **MIT** | 按许可条件处理 |
| CSC（CoPsyN/CSC）| 训练 | 源仓库声明 **MIT** | 按许可条件处理 |
| ToSarcasm（HITSZ-HLT/ToSarcasm）| 训练 + SFT + **DualTest 中文侧** | 所访问版本未识别到明确再分发许可 | 不再分发人类原文；直接发布须另获许可 |
| News Headlines v2（Misra, Kaggle/GitHub）| 训练 | **CC BY 4.0**（2026-08-02 核实：Kaggle 页面 JSON-LD `license.name = "Attribution 4.0 International (CC BY 4.0)"`, url creativecommons.org/licenses/by/4.0/, dataset id 30764；GitHub 侧无 LICENSE 文件）| 许可允许署名后再分发；本项目仍选择不分发人类原文（保守口径统一）|
| Open Chinese Internet Sarcasm Corpus（Zhu 2022）| 训练 | 原链接失效，未识别到明示再分发许可 | 不再分发人类原文 |

**当前发布决定（2026-08-02，覆盖 2026-07-21 的早期方案）**：本轮不上传或承诺发布 DualTest、代码或其他研究工件。若未来另行考虑发布，权利边界仍按上表执行：不得分发未取得明确再分发许可的人类原文；可发布内容也须重新核对模型条款、种子语料权利与署名要求。早期“仅发布 AI 文本、元数据与重建脚本”的方案不是当前承诺。

## 🔒 生效红线与口径速查（改论文前必读，勿回改）

**格式合规红线（ⓕ 2026-07-17，最高优先级）**：EACL/ARR/ACLPUB 的一切格式要求 = 红线，任何内容改动不得违反；**所有可能导致 desk reject 的要求同为红线**。执行标准：① 样式文件（acl.sty/acl_natbib.bst）零修改，且**不得在导言区覆盖样式文件的刻意设定**（如 `\flushbottom`——07-17 曾为消除 float 段间空白加 `\raggedbottom`，属灰色地带已回退；美观问题一律让位于合规）；② 版式/字体类改动必须有官方依据才可做（指南明文或官方模板自身做法——如 Times 主字体修复、XeLaTeX 下 preamble 设字体均有 acl_lualatex.tex/ACLPUB §Fonts 背书）；③ 硬指标清单：A4、边距、字号、正文 ≤8 页、Limitations 节必有且不含新内容、附录双栏、`[review]` 模式（行号+页码）、双向匿名（无致谢/自引第三人称/无可追踪链接）、Responsible Checklist 如实填写、无幻觉引用、附录不得塞正文内容；④ 拿不准是否合规的改动默认不做，先查 ACLPUB formatting.html / ARR CFP 原文再动手。

**正向贡献表达红线（2026-07-24）**：摘要、引言、结果主题句与结论必须以“确立了什么、缩小了什么范围、提供了什么后续目标”为落点；不得用连续的“未能 / 不能证明 / 没有识别 / 仅仅 / rather than”作为贡献声明的主要收束。必要的因果边界统一写成“证据支持 X，并将 Y 界定为下一步分析目标”。本红线不得削弱 AI 侧标签构造、数据划分与泄漏复核、模型选择、实验对照、Limitations 与 Ethics 中的事实披露，也不得把相关证据升级为未经支持的因果结论。

**术语可解释性红线（2026-08-02 当前覆盖）**：① 领域惯用或已有明确引用的方法名/缩写可以保留，不为形式统一强制展开，如 PPO、DIP、RCNN、AUC、LEACE、INLP、QLoRA、BH-FDR；② 本文自定义名称仅在确实指代反复使用的稳定对象、配置或核心现象，且首次出现立即定义时保留，包括 SarcTrain、AuthTrain、GenTrain、DualTest、task-branch interaction projection、INLP-3 residual PC1 与 class-conditional negative transfer；③ §5.4 以 Q1--Q3 组织三个诊断问题，Q3 下的 `(a)/(b)/(c)` 只是干预类别标题，不包装为新方法。旧称 `srcORTH-PC1` 已从当前 tex、附录、Figure 3 和表格中删除，不得恢复。普通实验说明必须直接说明“如何设置、观察到什么、证据支持到哪一步”。

**标签口径（ⓔ 2026-07-11 + 07-14；07-20 更新）**：AI 侧 `is_sarcastic` = 生成指令的**构造定义**。核心任务字段、四象限与指标继续使用 `is_sarcastic × is_human`、source $×$ sarcasm、AI $×$ non-sarcastic 和**误判率/FPR**，不全局改名为 sarcasm-reference / target-style 体系。`requested style`、`prompted condition`、`target` 仅用于局部说明 AI 标签来源，不替代任务与象限名称。**§3 与 Limitations 必须保留完整边界**：AI 侧讽刺标签记录 prompted condition，并非独立人工标注的 perceived sarcasm；摘要使用紧凑的正面事实表述“AI-generated text prompted to be non-sarcastic”，不必重复完整 caveat。AuthTrain 中作者来源标签与指令风格标签是分别构造、近似平衡的因素，**不假设实现后的文本特征或感知讽刺性独立**。单头 19.3% 反事实继续作为标签质疑的核心对照。2026-06 封存令维持（人类侧标签异构性不讨论）。

**因果口径**：失效"**由联合训练诱导**（induced by joint training）"不得降级为"相关/associated"（证据 = 同文本单头反事实 + McNemar）。

**机制证据口径（2026-08-02 当前覆盖）**：Q1 回答来源是否可读，Q2 回答来源相关分数和数据加权方向是否与受影响决策共同变化，Q3 回答表征干预是否改变冻结讽刺头在两个来源组之间分配误报的方式，以非讽刺文本上的来源组 FPR gap 为读出。LEACE 与 INLP-3 residual PC1 都会缩小 gap，但两者的 residual source AUC 分别为 0.589 和 0.982，因此 gap 不随线性来源可解码性一致变化。INLP-3 residual PC1 是在 fitting half 上移除三条 INLP 方向后，对残余空间做 PCA 得到的最高方差轴；evaluation 时只从原始表征移除该 PC1，并不同时移除 INLP-3 子空间。不得把它称为已识别的来源特征、修复或唯一因果机制。**禁** source causes failure / model relies on source / “证明捷径” / “假说失败”。

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

**答辩弹药（专家四问 + 标签）**：① 为何讨论来源相关结构→Q2 的 logit 相关和协方差感知方向对齐将其与受影响决策联系起来；② LEACE 与 INLP-3 residual PC1 为何都重要→前者同时降低 gap 与 source AUC，后者在 source AUC 仍为 0.982 时缩小 gap，说明二者不必同步；③ 与主任务关系→AI×非讽刺是实际输入子群，高误判是鲁棒性问题；④ 为何不作唯一因果归因→当前干预识别的是几何敏感性，语义特征与跨种子稳定性仍需后续实验。标签质疑→单头 19.3% 对照 + prompt-specified style 的构造定义口径。

## 📖 已解决历史归档（决策日志，倒序；其中页数、表号和章节号只代表当日版本，不是当前状态）

- **07-30（第五章局部措辞与结构优化）**：不再声称 DeBERTa-v3-large 预先指定或由 DualTest 选出，Table 1 改为“不应解释为独立架构选择实验”；明确 ten comparisons 均为讽刺任务的 fold-matched 比较，并直接说明 MGTD 没有对应的一致下降。§5.3.2 将干预协议与 LEACE 结果拆段，首次报告 AI/人类比率时改用完整句，`High-variance control` 改为 `High-variance sensitivity test`，协方差对齐改述为“在观测表征协方差结构下评估”。Qwen3.5 段明确 FPR 对应生成的非讽刺子群。未添加无法核实的 `prespecified` 或 `two-sided`；编译仍为 21 页，§6、Conclusion 与 Limitations 位于 p.8。

- **07-30（第五章逻辑保持型压缩）**：在不删除核心证据链的前提下，以合并句子和表格—正文分工压缩第五章。5.1 删除标准差及语言 F1 复述，并明确 Table 1 不用于 DualTest 模型选择；5.2 保留 all ten、−6.08/−4.43 与 MGTD 对照及完整联合流程边界；5.3.1 保留 matched 设计、67.0/14.8 pp、两组 McNemar $p<.001$、AUC/中位数、人类—AI 与中英文分层，discordant counts、IQR 和 MGTD 头精确率留附录；5.3.2 保留 probe 0.986/0.974、$\rho=0.519$、普通/协方差几何、互斥样本半集、冻结讽刺头、评估半 re-probe、LEACE 0.589 与 19.1-pp gap、阴性控制、srcORTH-PC1 12.6-pp gap/0.982 及因果边界；5.3.3 保留未经重训的 Qwen3.5 plain/RCNN matched 结果。三稿同步后构建恢复为 21 页，Conclusion 与 Limitations 位于 p.8，无 overfull 或未定义引用。

- **07-30（§5.3.2 完整逻辑扩写，待压缩）**：纠正 “one half of the frozen representations” 的歧义：实际切分的是 DualTest 样本而非表征维度，每个样本保留完整向量；完整数据 probe/相关/对齐使用全部适用样本，只有干预采用互不重叠的拟合半与评估半。正文明确拟合半估计方向/变换、评估半使用原讽刺头计算 FPR，并在评估半内部对新来源 probe 做五折交叉验证；补入 84.8% 与 86.3% 的样本范围区别。方向对齐增加 $|\cos(\Sigma^{1/2}w_1,\Sigma^{1/2}w_2)|$ 公式、无标签全测试集协方差及传导式证据边界；LEACE 增实现检查、held-out re-probe、错误均值与单方向/INLP/随机阴性结果；srcORTH-PC1 增逐步构造和非来源特征边界。三稿及 Appendix E.1 同步。当前为 22 页扩写稿，Conclusion p.9，下一阶段再压回 p.8。

- **07-30（第 5 章正文自足性补强）**：§5.3.1 增加朴素配对目标子群的概率中位数/IQR（0.767 [0.635, 0.839] vs. 0.204），将“上移”限定为与分布证据一致；正文直接报告跨语言差值（朴素：英 +63.5 pp、中 +79.4 pp；RCNN：+16.9/+7.7 pp）及语言—语料混杂边界。§5.3.2 明确三个诊断问题与单个 final plain joint model 的适用范围，定义五折交叉验证逻辑回归来源 probe，解释普通余弦与协方差归一化对齐的差异，并将 held-out 干预结果直接组织为 AI–人类非讽刺 FPR gap（52.0→19.1 pp，srcORTH-PC1 后 12.6 pp）；保留 LEACE 后重训 probe、来源方向/INLP 阴性结果及 srcORTH-PC1 的非来源特征边界。§5.3.3 增补 Qwen3.5 上配对 RCNN 22.1%/10.6% 对照。未把复现公式和完整曲线移入正文；全量构建仍为 21 页，Conclusion 与 Limitations 均在 p.8 开始/结束于要求范围内。

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

## 与 Markdown 长版稿的关系

- `manuscript_eacl2027.tex` 与 `appendices_eacl2027.tex` 是投稿文本、数值、表号和章节定位的唯一权威源。
- `manuscript_eacl2027_en.md` 与 `manuscript_eacl2027_zh.md` 仅作长版阅读和语言参考，不保证与 2026-08-02 投稿快照逐句同步；不得据其覆盖 tex。
- OpenReview 标题与摘要使用 `openreview_metadata.txt`；当前已与 tex 逐字核对。Responsible NLP Checklist 与答辩口径分别以本文件的当前区段和 `rebuttal.md` 为准。
