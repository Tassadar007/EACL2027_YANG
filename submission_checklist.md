# EACL 2027 投稿合规清单与待办

> 依据：https://2027.eacl.org/calls/papers/ 与 https://aclrollingreview.org/cfp（2026-07-03 查证）。投稿路径：**ARR（OpenReview）→ EACL 2027 承诺（commitment）**。
> 本文件 = 投稿工作总账。结构：① 待办 ② 构建状态 ③ 日期/合规/Checklist ④ **生效红线速查（改论文前必读、勿回改）** ⑤ 已解决历史归档。

## 🔴 投稿前待办（TODO，按优先级）

1. **OpenReview/ARR 表单**：Responsible NLP checklist（草稿见 §Checklist）、preprint/匿名选择、AI 写作披露、EACL commitment 字段。
2. **全体作者 ARR 审稿注册**（2026-08-05 前，强制）。
3. **Mac 本地重编译终检**（07-15 沙盒已全跑通，见构建状态；Mac 重编译后再跑一次 `pdffonts` 确认主字体为 TimesNewRomanPSMT）。
4. **数据许可**：确认五源可再分发；匿名代码/数据用 Anonymous GitHub（禁 Dropbox/Drive；Open Chinese 无有效链接、经期刊 Zhu 2022 获取）。
5. **（可选）** 附录 A.4/D 中文示例加拼音转写。
6. OpenReview 提交（ARR 2026-08 周期）→ 10 月 EACL 承诺。

（原"英译""ACL 模板排版""9 条文献核实""aclpubcheck/pdffonts/匿名化/元数据终检"已完成，见构建状态与 07-15 归档。）

### ✅ 文献核实结果（07-15 逐条对原始出处核毕，custom.bib 已修）

- **Bevendorff 2025** ✅ 与 ACL Anthology 完全一致（Findings ACL 2025, pp.3762–3787, doi:10.18653/v1/2025.findings-acl.194）；正文"frame as authorship verification"表述与摘要相符。
- **Chai & Wang 2025** ✅ CVPR 2025 openaccess 完全一致（Chai, Lu, Wang, pp.25698–25707）。
- **Compton 2023** 🔧 已修：venue 原写 CHIL，实为 *Proceedings of the 8th Machine Learning for Healthcare Conference*（PMLR 219），补 pages 110–127、publisher PMLR。
- **Du 2022（PALI-NLP）** 🔧 已修：作者原列 6 人（含 Jin Meizhi、Mo Yang），Anthology 正典为 5 人：Du, Hu, **Zhi (Jin)**, Jiang, Shi。
- **Li 2023** ✅ EMNLP 2023 完全一致（pp.16460–16476）；补 doi。
- **ViSP** ✅ 期刊版确认：*Neurocomputing* 678:133185, doi:10.1016/j.neucom.2026.133185，作者 Changli Wang, Fang Yin, Jiafeng Liu, Rui Wu（arXiv 版仅 3 作者，期刊版含 Jiafeng Liu，勿按 arXiv 改）。
- **SarcNet（Yue 2024）** ✅ LREC-COLING 2024 完全一致（Yue, Shi, Mao, Hu, Cambria, pp.14325–14335）。
- **Ye** 🔧 已更新为正式版：*The Clever Hans Mirage: A Comprehensive Survey on Spurious Correlations in Machine Learning*, **TMLR 2026**（OpenReview kIuqPmS1b1），作者扩至 19 人；bib key `ye2024spurious` 不变，渲染变为 "Ye et al., 2026"。

## 📄 LaTeX 构建状态（tex = 投稿实体）

**文件**：`manuscript_eacl2027.tex` + `appendices_eacl2027.tex` + `custom.bib` + `figures/`。**当前标题**：*When Joint Training with Machine-Generated Text Detection Can Distort Sarcasm Detection: A Bilingual Diagnosis of Class-Conditional Negative Transfer*。**编译**：`xelatex → bibtex → xelatex → xelatex`（中文需 XeLaTeX）。07-20 全文逻辑与行文审校后，输出 `manuscript_eacl2027.pdf` 共 **20 页**；正文 §1–Conclusion 位于 p.1–8，Limitations/Ethical considerations/References 从 p.9 起，附录从 p.13 起。

**⚠️ 主字体修复（07-15，勿删；合规依据 07-15 已逐项核证）**：ACLPUB formatting.html §Fonts 原文："All text (except non-Latin scripts and mathematical formulas) should be set in **Times Roman**. If Times Roman is unavailable, you may use **Times New Roman** or **Computer Modern Roman**."——Times New Roman 为明文允许字体；官方仓库自带的 XeLaTeX/LuaLaTeX 模板 `acl_lualatex.tex` 本身就在 preamble 用 `\babelfont{rm}{TeXGyreTermesX}` 设置拉丁主字体（"similar to Times"），故在文档 preamble 设主字体=官方做法，非"修改样式文件"（acl.sty/acl_natbib.bst 未动）；CJK（非拉丁文字）与数学公式明文豁免于 Times 要求。xeCJK/fontspec 会把拉丁主字体静默重置为 Latin Modern，覆盖 `\usepackage{times}`——旧 PDF 因此主字体错误（aclpubcheck 报 Wrong font）。已在 preamble 的 xeCJK 块后加 `\IfFontExistsTF{Times New Roman}{...}{\setmainfont{TeX Gyre Termes}}`：Mac 上取 Times New Roman（PDF 字体名 TimesNewRomanPSMT，aclpubcheck 接受），无该字体环境回退 TeX Gyre Termes。**07-15 终检结果**：pdffonts 全嵌入 ✅；PDF 元数据无作者名 ✅；匿名化搜索（our previous/我们前期/Acknowledg/作者名）仅命中 References 第三人称自引 ✅；aclpubcheck 除 review 模式行号导致的 spurious margin 报错外，唯一真实错误即上述字体（已修，Mac 重编译后应消失）；无 undefined citations、无 overfull。

**格式合规（已过）**：正文 §1–Conclusion 在 8 页内（Conclusion p.8）、Limitations/Ethical considerations/References 从 p.9 起、附录 p.13；A4、字体全嵌入（含 Noto CJK）；`[review]` 模式（行号+页码+匿名）；两栏；Limitations/Ethical considerations 不编号、Conclusion 后 References 前；附录双栏、字母编号 A–E；无未定义引用/重复标签/overfull。**tex 摘要 190 词**。07-20 审校将各论证部分统一为“目的/问题 → 实验或证据 → 局部结论”，拆分段内话题切换和过长句；并将联合任务动机明确为“来源对讽刺是干扰变量、对 MGTD 是预测目标”的表示冲突。摘要按“现实背景 → 两项判断及生成侧用途 → 研究缺口 → 研究目的 → 高层数据与诊断方法 → 一句发现与意义”组织，不列具体结果数字；AI 侧在摘要中以“prompted to be non-sarcastic”紧凑标明构造条件，完整的人工标注边界留在数据章节与 Limitations。结合 `prism-uploads/deep-research-report.md` 时仅采用与现有证据一致的叙事建议；未加入 AI 润色、对抗攻击、联合效用或广泛部署等未经本文实验检验的主张。实验数字、事实、限定语及结论强度均未改动。

**tex 相对 md 的压缩（勿当不一致回改 md；md 保持较全权威源）**：贡献 4 条→三句一段（单一 softened "first"）；§2.4 收一段；§5.3.2 机制压两段 + **表 4 删**（数字全在附录 E）；~~主干对比表→附录 C.1；四象限图→附录 C.7~~（**07-15 用户决策撤销此两项**：正文有余量，主干表移回 §5.1、四象限图移回 §5.3.3，附录 C.1 只剩宏平均 F1 补充、C.7 只剩表格与配对检验）；§6→短节 "Detector–Judge Misalignment"（Spearman/逐评审/语言/案例全下沉附录）；结论一段；Limitations 改"承认而非再辩"；"five independent runs"→"five fold-based runs"。检索脚注、reward/RL future work、附录 E.2 指针已按 07-14 决策补齐。07-15 结论段补回 SFT 语言不对称一句（"The generation analysis additionally reveals a language asymmetry after SFT; …"），与 md 一致，勿当压缩差异回删；同日决策：结论保持一段，不重申 DualTest/双头贡献（避免第二处 novelty claim），future work 维持在 Limitations。**tex/en md 表号（07-15 移回后）：主干对比=表 1、双头vs单头=表 2、四象限=表 3、Spearman=表 4；架构=图 1、四象限图=图 2（正文 §5.3.3）**（zh md 表号仍自成体系）。**07-15 移回事故与修复**：首次移回时提取脚本误吞附录 A.2–C.1 整段入正文（编译件曾出现附录变正文第 6/7 章、18 页超标），已完整回退并重做——本次仅外科手术式移动两个浮动体本身，已静态验证：正文恰 7 个 \section、附录 A–E 结构与段序完整、table/figure 环境配平、tab:backbone 与 fig:quadrant 仅存于正文。**07-15 §4.1/§5.1 充实与代号清除（三稿同步）**：ⓐ §5.1 补一句三类编码器的预训练特征（mBERT=多语言维基 BERT-base / XLM-R-base=百语 CommonCrawl / DeBERTa-v3=解耦注意力+ELECTRA 式、英语为主）；ⓑ 附录 B.3/B.4 的两个中间层变体结构讲解（含维度、交互投影公式与**注意力权重恒为 1 的退化说明**）整体上移并入 §4.1——退化说明就是"为何不叫 cross-attention"的答案，正文现以否定形式出现一次 "rather than as a form of cross-attention"，属刻意，勿删；ⓒ **B4/B5 实验代号全文清除**（表 1 行名、附录 B.1 学习率表行名、附录段题），只用正式名 RCNN / task-branch interaction projection；原附录 B.5(SFT) 改号 B.3，已验证无悬空引用、无代号残留；ⓓ §5.1 重排为"先设置后结果"两段（设置段=比较范围+编码器特征+F1 口径，结果段=表 1 数字与结论；三稿同步，zh/en 的表格移至两段之间）——已核查其余实验小节（§5.2/§5.3.1/§5.3.3/§6）开头本就是先协议后结果，无需动；ⓔ preamble 新增 \Tabref/\Figref 宏并全量替换 Table~\ref/Figure~\ref，Table/Figure 字样纳入链接热区（与 \Sref/\Appref 同机制）；ⓕ 贡献描述位去防御化（07-15 用户指示，仅 tex——三处均为 tex 压缩时引入的对 zh 正面措辞的偏离，本次改回即恢复忠实）：结论 "but do not establish source identity as its causal mechanism"→"characterize … and delimit the remaining step to causal attribution"；§1 删 "and do not isolate source identity as its cause" 从句（边界由紧随的 hypothesis 定位句承担，zh/en 本无此句）；摘要 "rather than a causal dependence on source"→"with the source concept's specific causal role remaining a hypothesis"（对齐 zh"保留为待检验的假说"）。**保留不动**：§5.3.2 证据句 "but do not establish source identity as the causal feature"（该位置的一句非声明配额）与 Limitations 全部 scoping（法定豁免区）；claim 强度三处均未变。**待办：Mac 编译核实正文（§1–Conclusion 含新表新图与 §4.1 扩写）仍在 8 页内**——表+图净增约 0.4 页，结论原止于第 8 页约 1/4 处，应有余量但须以编译 PDF 为准；若超页，优先回退四象限图（en md 同步回退 Figure 2 与表号）。

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
| 论文类型 | 长文正文 ≤ 8 页（录用后 +1）| ✅ tex 正文在 8 页内 |
| 不计页内容 | Limitations、Ethics、References、附录 | ✅ 结构已排（Limitations 在结论后、References 前）|
| **Limitations 强制** | 缺失即拒稿；不得含新方法/分析/结果 | ✅ 仅复述正文边界 |
| 模板 | 官方 ACL 样式，禁改样式；附录双栏 | ✅ 用 acl.sty `[review]`、附录双栏；投稿前跑 aclpubcheck |
| **双向匿名** | 无作者名/致谢；自引第三人称；补充材料匿名 | ✅ 前序工作 Yang & Ikeda 2023/2024 第三人称；全稿无"我们前期"|
| 预印本 | 若勾 "no preprint" 则不得预印 | 投稿时选择 |
| Responsible NLP Checklist | 表单填写；不实可拒稿 | ✅ 草稿见下 |
| 双重投稿 | 禁止 | 确认无并行投稿 |
| 审稿义务 | 全体作者 08-05 前注册 | ⚠️ 待办 |
| 论文完整性 | 禁幻觉引用 | ✅ 9 条已逐条核毕（07-15），4 处修正见"文献核实结果" |
| AI 写作辅助 | 允许但须披露 | ✅ Ethics 已披露 |

## Responsible NLP Checklist 答案草稿

- **A（局限与风险）**：有 Limitations 节；风险讨论在 Ethics（检测器误用、子群系统性偏差）。
- **B（数据/工件）**：用公开数据集（iSarcasm、CSC、ToSarcasm、News Headlines v2、Open Chinese），五源已正式引用、遵循原始许可（Open Chinese GitHub 已失效、仅存期刊 Zhu 2022，数据可得性表述避免失效链接）；新工件（DualTest 等）为衍生数据、含 AI 生成文本带来源标注；文档见 §3 + 附录 A；无新采个人数据；匿名期发数据用 Anonymous GitHub。
- **C（实验）**：报告架构维度、超参与选择依据（§4 + 附录 B，类别权重与采样比先验设定、未在测试集调参）、统计描述（五次运行均值±std；Wilcoxon+BH-FDR、Wilson/McNemar，附录 C）、包版本（附录 B：torch 2.9.0/transformers 4.57.1/peft 0.18.0/RTX 3090 ✅）。
- **D（人工标注/被试）**：无人类被试；LLM 盲评为 API 自动调用（§6 + 附录 C.3）。
- **E（AI 助手）**：写作润色/格式用 AI 辅助；构思、实验、分析由作者完成（Ethics 已披露）。

## 🔒 生效红线与口径速查（改论文前必读，勿回改）

**格式合规红线（ⓕ 2026-07-17，最高优先级）**：EACL/ARR/ACLPUB 的一切格式要求 = 红线，任何内容改动不得违反；**所有可能导致 desk reject 的要求同为红线**。执行标准：① 样式文件（acl.sty/acl_natbib.bst）零修改，且**不得在导言区覆盖样式文件的刻意设定**（如 `\flushbottom`——07-17 曾为消除 float 段间空白加 `\raggedbottom`，属灰色地带已回退；美观问题一律让位于合规）；② 版式/字体类改动必须有官方依据才可做（指南明文或官方模板自身做法——如 Times 主字体修复、XeLaTeX 下 preamble 设字体均有 acl_lualatex.tex/ACLPUB §Fonts 背书）；③ 硬指标清单：A4、边距、字号、正文 ≤8 页、Limitations 节必有且不含新内容、附录双栏、`[review]` 模式（行号+页码）、双向匿名（无致谢/自引第三人称/无可追踪链接）、Responsible Checklist 如实填写、无幻觉引用、附录不得塞正文内容；④ 拿不准是否合规的改动默认不做，先查 ACLPUB formatting.html / ARR CFP 原文再动手。

**标签口径（ⓔ 2026-07-11 + 07-14；07-20 更新）**：AI 侧 `is_sarcastic` = 生成指令的**构造定义**。核心任务字段、四象限与指标继续使用 `is_sarcastic × is_human`、source $×$ sarcasm、AI $×$ non-sarcastic 和**误判率/FPR**，不全局改名为 sarcasm-reference / target-style 体系。`requested style`、`prompted condition`、`target` 仅用于局部说明 AI 标签来源，不替代任务与象限名称。**§3 与 Limitations 必须保留完整边界**：AI 侧讽刺标签记录 prompted condition，并非独立人工标注的 perceived sarcasm；摘要使用紧凑的正面事实表述“AI-generated text prompted to be non-sarcastic”，不必重复完整 caveat。AuthTrain 中作者来源标签与指令风格标签是分别构造、近似平衡的因素，**不假设实现后的文本特征或感知讽刺性独立**。单头 19.3% 反事实继续作为标签质疑的核心对照。2026-06 封存令维持（人类侧标签异构性不讨论）。

**因果口径**：失效"**由联合训练诱导**（induced by joint training）"不得降级为"相关/associated"（证据 = 同文本单头反事实 + McNemar）。

**机制 = 修正假说**：口径 = "部分支持并限定来源捷径假说；失效更宜刻画为与来源共线、与风格纠缠、位于主导方差子空间的捷径，非对来源身份的简单因果依赖"。用 source-correlated / style-entangled；**禁** source causes failure / model relies on source / "证明捷径" / "假说失败"。srcORTH-PC1 正交对照（擦正交高方差方向同样降误判、来源信息不动）= 本文发现与严谨性优势。每处仅一句非声明。方法引用挂靠：INLP=Ravfogel 2020、LEACE=Belrose 2023、选择性=Hewitt & Liang 2019、amnesic=Elazar 2021。

**术语（英译/全文）**：plain dual-head / single-head（禁 naive）；task-branch **interaction projection**（禁 cross-attention / cross-projection）。其正文定义以残差投影公式 $h_s'=h_s+P_{a\rightarrow s}(h_a)$ 及对称式为准；单向量设置中原 attention 权重恒为 1，不存在 query–key 交互。07-15 的原句 “rather than as a form of cross-attention”已由 07-20 的公式化定义取代，冻结的是计算语义而非该字面句子。正式任务名统一为 **machine-generated text detection (MGTD)**，中文为“机器生成文本检测”，任务头/指标写 MGTD head / MGTD F1；具体样本写 AI-generated text；`authorship verification` 仅用于 Bevendorff 等范式讨论；禁 AI-authorship detection、authenticity/veracity；“类人度/human-likeness”仅 §6 盲评维度；**class-conditional failure**（禁 OOD failure；§2.3 引 Mueller"分布外"是文献描述、可留）；**跨生成器复现**（禁 cross-family；deepseek-r1:8b 是 Qwen 基座非 Llama）。

**数据集名**（投稿版；发布代码/数据对齐，长版/JSON 用内部名）：SarcTrain=D_critic_sarcasm(10,000 全人类)/AuthTrain=D_critic_auth(19,995 = 9,999 人+9,996 AI)/GenTrain=D_actor_sft(2,428)/DualTest=D_test_dual(4,664)。"Master Metadata"→"标准化合并后的源数据总集"。

**§6 定位**：与 §5"**相关但独立的问题**；§5 失效是失配的一个已识别来源，但非全部"；禁强因果"缺陷传导为失配"；**禁"human perception/人类感知"**（三 LLM 盲评 = 人类判断的代理）。reward 三段弧线：动机（两连续分数原理可作评价信号 / ViSP 先例，不否定 ViSP）→ ρ 证伪 → future work 纳入奖励设计；**不得声称跑过 RL**。

**写作三原则（全文）**：① 正面措辞（确立了什么 + 还差什么工具；不堆叠"未能/不主张"）；② 零伏笔（§3/§4 只陈述执行、不预演失效；协议差异按流程平铺、不用"理由"框架）；③ 去辩护化（拒绝辩护姿态、总分结构、只给正面理由）。章开头不念目录、不预报结果数字。

**数字口径**（07-11 已逐项对账，勿写回旧值）：§3 人类侧 **1,400 英/972 中**（非 1,423/949）；§5.1 方差减小 **约 34–47%**（非 35–50%）；参考盲评均分 **3.77**（非 3.78）；白化对齐 **|cos| 0.41（probe 方向）/0.49（真实性头方向）分开报**（勿恢复"≈0.45 两方向一致"）；probe **0.986 / 置换 0.512**（单头 0.974）。§5.3 四象限用各自 **final 模型点估计**（非 5 折模型）。probing 脚本在 `scripts/sft_eval/probing/`、记录 `paper3/probing_findings.md`。

**引用口径**：P1/P2 = Yang & Ikeda 2023(IIAI-AAI,1–6)/2024(IJSKM 8(2)) 第三人称；ViSP = 期刊版 *Neurocomputing*（勿写 arXiv 2025）；CSC = Jang & Frassinelli 2024 + Jang, Braun & Frassinelli 2023（双引）；ToSarcasm = Liang et al. 2022 CCL 557–568；Open Chinese = Zhu 2022（勿给失效 GitHub）。⚠️ Jang 2023（intended vs perceived）仅作数据集引用，勿重开标签口径讨论（2026-06 封存）。

**不采纳（勿被审稿/修订稿推回）**：标签改名 / 因果降"相关" / 人工标注（无志愿者）/ 用 3-LLM 核验 AINS 标签（留 rebuttal）/ 跨运行象限逐折统计（final 即 k-fold 产物）/ MLM 继续预训练 / 预算匹配 / 采样比敏感性 / 梯度冲突测量 / B-W2 多语言编码器 probe 复现 / W4 数据增广基线 / 泄露删样重训（改报条数）/ 评价者修订稿整体弱化姿态。

**答辩弹药（专家四问 + 标签）**：① 为何仍叫 source shortcut→标题摘要已用弱措辞；② srcORTH 有效否定 LEACE？→正是本文发现、严谨性优势；③ 与主任务关系→AI×非讽刺是部署真实输入，高误判 = 鲁棒性问题非噪声；④ 为何不做因果中介→目标是识别刻画、留 future work。标签攻击→单头 19.3% = 标签噪声上界论证 + 构造定义口径。

## 📖 已解决历史归档（决策日志，倒序；细节可查 git / 记忆 `project_paper3_writing_dir.md`）

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
