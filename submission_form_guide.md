# ARR 2026-08 周期投稿表单填写指导（v4，2026-08-02 当前稿）

> **v4 同步说明**：本版已与当前 `manuscript_eacl2027.tex`、`appendices_eacl2027.tex`、项目 PDF 及作者提供的实际 OpenReview 字段对齐，包括 §5.4 的 Q1--Q3 结构、`INLP-3 residual PC1` 名称、170 GPU-hours、News Headlines v2 的 CC BY 4.0、保守的 C2 答案，以及当前表单唯一的 A1 Potential Risks 问题。答案按**问题内容**归档，不按其他版本说明页的编号硬套。
>
> **一条总原则：表单页面 > 本文件 > 官网说明页。** 若本文件与你眼前的表单字段不符，以表单为准，不要硬套编号。
>
> 🔴 **当前提交快照**：项目 PDF 共 25 页，正文与 Conclusion 完整结束于 p.8，Limitations 从 p.9 开始；Figure 3 已使用 `INLP-3 residual PC1`，不再含 `srcORTH-PC1`。当前 PDF SHA-256：`0e6f38f952ff7d6a888a20b66661e3bad1bc3e57946f6ea2a1aeff70e32bbc5b`。若此后再改 tex、附录、图片或参考文献，该校验值立即失效，必须重新全量编译和检查，不能提交旧 PDF。
>
> 🔴 **不做代码或数据等研究工件的发布承诺**（作者决定，2026-08-02）：本次不提供代码与数据，也不在 checklist 或附件说明中声称将来会发布。附录 A.1 现有的"可再分发范围"属条件性说明，不是承诺，保持原样即可。Preprint Status 是另一项独立政策问题，按第 3.3 节如实选择。

---

## 0. 三个硬截止（AoE = UTC-12，比日本时间晚 21 小时）

| 时间 | 事项 | 谁做 |
|---|---|---|
| **2026-08-03 23:59 AoE**（日本时间 08-04 20:59） | 在 OpenReview 提交论文 | 第一作者 |
| **2026-08-05 EoD AoE** | 两位作者各自完成审稿注册表 | 本人 + 老师 |
| 2026-10-11 23:59 AoE | EACL 2027 commitment | 第一作者 |

⚠️ 审稿注册的 48 小时是**从投稿总截止时刻起算**（08-03 → 08-05），**不是**从你个人提交论文那一刻起算。早交不等于早到期，晚交也不会顺延。

---

## 1. 老师需要做的两件事

### 【事项 1｜提交前】确认 OpenReview 资料完整

- 无账号：https://openreview.net/signup ｜ 已有账号：https://openreview.net/profile

表单在添加作者时**可以用姓名或邮箱搜索**，也允许临时添加尚未注册的作者，所以不是"没账号就填不进去"。但 ARR 要求所有作者的 profile 完整，缺项会影响审稿分配与合规核查。需要填全：

- 姓名（与已发表论文一致）／现单位 + 起始年份／**DBLP** 链接／Semantic Scholar 链接／ACL Anthology 链接／可正常收信的邮箱

### 【事项 2｜提交后】完成审稿注册表（**08-05 EoD AoE 前**）

- 链接在论文提交后出现在作者控制台，同时 ARR 发邮件。控制台：https://openreview.net/group?id=aclweb.org/ACL/ARR
- ARR 自 2025 年 5 月起要求**每一位署名作者**注册为审稿人。**未按时完成注册或未完成 checklist 的论文存在直接被 desk reject 的风险**——这是全流程最容易因"以为对方会填"而翻车的一项。
- 注册 ≠ 一定被分配稿件，但注册本身不能省。
- 审稿人须知：https://aclrollingreview.org/reviewerguidelines
- 需要豁免（在其他 NLP 会议任 AC/SAC、医疗紧急、育儿假等）：**仍要填那张表**，在表里选豁免理由。政策：https://aclrollingreview.org/exemptions2025
  不被接受的理由：行政职务、休假、给别的会议审稿、换工作、太忙。

> 第一作者代劳不了这一步。提交完成后当天就把控制台链接发给老师。

---

## 2. 我（第一作者）提交前必须做的

1. **重新用 XeLaTeX 全量编译**。
   当前项目 PDF 已在 §5.4 改为 Q1--Q3 并更新 Figure 3 后重新构建。检查结果：正文 §1--Conclusion 完整止于 p.8，Limitations 从 p.9 开始；无 Overfull 或未定义引用；主字体为 TimesNewRomanPSMT，23 个字体对象全部嵌入；PDF 元数据无作者字段；`[review]` 模式行号、页码和双栏正常。
   上传前再核对文件 SHA-256 是否仍为本文件顶部记录的值；若不同，重新执行同一套检查。
2. 确认自己的 OpenReview 资料同样完整。
3. 摘要粘贴 `openreview_metadata.txt` 的纯文本版（已与 tex 逐字核对，192 词），不要手打。

---

## 3. 实际表单字段逐项

> 以下按论文修改者提供的**实际字段名**组织。若页面上还有本表未列的必填项，先停下来问，不要猜填。

### 3.1 论文本体

| 字段 | 填什么 | 说明 |
|---|---|---|
| Title | 与 tex 逐字一致 | |
| **Authors** | 两人：**第一作者在前，老师在后**。可用姓名或邮箱搜索添加 | **截止后作者名单冻结**，提交前务必确认人数与顺序 |
| Abstract | 粘贴 `openreview_metadata.txt` | 192 词，已与 tex 逐字核对，不要手打 |
| **TL;DR** | 见下方备好的文本 | 必填，一两句话 |
| PDF | 最终匿名 PDF | 见第 2 节；**必须是重编译后的新 PDF** |
| Paper Type / Length | **Long paper** | 正文 8 页 |
| **Research Area** | **Interpretability and Analysis of Models for NLP**（备选 Resources and Evaluation） | 本文核心是"联合训练为何失效"的诊断与表征分析，选分析类轨道能匹配到懂 probing / negative transfer 的审稿人 |
| **Keywords**（自由关键词） | 见下方，**逗号分隔** | 与下一项是**两个不同字段**，不要混填 |
| **Research Area Keywords**（官方受控词表） | 从该 Area 的**下拉/预设词表**里选，见下方建议 | 这是官方 area-specific 关键词，**不是自由关键词**；只能选表内已有的词，本文建议的三项若表内无同名，选语义最接近的 |
| **Languages Studied** | **Chinese, English** | 事实 |
| **Preferred Venue** | **EACL** | ARR 表单确有此必填项。它只是给 ARR 做分配规划的偏好声明，**不等于已投 EACL**——真正的投稿仍需 10-11 前单独提交 commitment |
| **License** | **CC BY 4.0** | ARR 要求为投稿本身选择许可；CC BY 4.0 是 ACL 系列的标准选项 |
| Contribution Types（多选） | ✅ Model analysis & interpretability<br>✅ NLP engineering experiment<br>✅ Data resources<br>✅ **Data analysis**<br>❌ 不勾 Publicly available software and/or pre-trained models | 加 Data analysis：本文大量子群分层与表征分析正属此类。不勾软件项：本轮不上传代码，勾了即与事实不符。<br>⚠️ **Data resources 与"本轮不提供数据"存在张力**，审稿人可能追问资源如何获取——附录 A.1 已写明重建路径与不再分发的理由，可据此回应 |

**TL;DR（可直接粘贴）**
> Adding a machine-generated-text detection head to a sarcasm detector can make it label most non-sarcastic AI-written text as sarcastic; we build a bilingual dual-labelled test set, show the failure is class-conditional and replicates across generators and languages, and use representation diagnostics to link the error pattern to source-related scores and directions.

> 📌 措辞刻意避开 "trace it to"／"caused by"：本文确立的是关联、方向对齐与干预敏感性，**不主张唯一因果归因**，TL;DR 不能比正文强。

**Keywords（自由关键词，逗号分隔，可直接粘贴）**
> sarcasm detection, machine-generated text detection, multi-task learning, negative transfer, shortcut learning, model interpretability, multilingual NLP

**Research Area Keywords（从官方词表中选，建议对应这三类）**
> data shortcuts/artifacts, probing, robustness

### 3.2 重投与重分配

实际表单**没有**简单的 "is a resubmission?" 开关，而是通过下面两个必填字段表达。首次投稿两项都选"不是重投"：

| 字段 | 选 / 填 |
|---|---|
| Reassignment Request Area Chair | `This is not a resubmission` |
| Reassignment Request Reviewers | `This is not a resubmission` |
| Previous URL | **留空** |
| Response to reviewers / Explanation of revisions | **留空** |
| 其余重投说明类字段 | **全部留空** |

**判定"是否首次投稿"不能只看两位作者的提交历史**。要确认的是：**这篇论文的任何标题或任何版本**是否曾被提交过 ARR——包括被 desk reject 的、主动撤回的、以及由其他作者账号提交的版本。ARR 规定即使旧版被 desk reject 也必须申报。据我们所知本文从未提交过，故两项均选"不是重投"；提交前请老师也确认一次。

### 3.3 预印本相关（**三个不同字段，别混**）

| 字段 | 填 | 说明 |
|---|---|---|
| **Preprint**（是否让 ARR 把本文作为**匿名预印本**公开发布） | **No** | 这只是让 ARR 把你的匿名 PDF 挂在 OpenReview 上公开。好处仅是占时间戳，对本文没有实际价值，还让稿件在评审期公开可见 |
| **Existing Preprints** | **留空**（我们目前没有任何预印本） | 若将来贴了 arXiv，那是提交之后的事，此处按提交当下的事实填 |
| **Preprint Status** | 四选一，见下 | 这才是关于**非匿名**预印本的政策承诺字段 |

**Preprint Status** 是**四选一的单选**，不是 Yes/No。请在页面上按实际选项文字选择：

- **推荐选择"正在考虑发布非匿名预印本"那一项**（形如 *We are considering releasing a non-anonymous preprint…*）。
- 理由：我们目前没有预印本，也不想现在就放弃将来贴 arXiv 的自由。**选"绑定的不发布（binding no）"是对自己的硬约束**——一旦选中，在元审查发布前贴出非匿名预印本即构成 desk reject 事由。既然将来可能想贴，就不能选它。
- ARR 自 2024-02-15 起已取消匿名期，评审期间贴非匿名预印本本身不违规，所以保留自由没有代价。
- ⚠️ 前提是我们**确实在考虑**将来发预印本。如果确定永远不发，才应如实选 binding no。

### 3.4 同意与协议类

| 字段 | 建议 | 说明 |
|---|---|---|
| Consent To Share Data | Yes | 用于 ARR 的同行评审研究与流程改进，无实质风险 |
| Consent To Share Submission Details | Yes | 与后续 EACL commitment 的信息流转有关，选 No 可能给自己制造麻烦 |
| Author Submission Checklist | 逐条读完再勾 | 这是作者自我声明，勾了就要负责 |
| Blind Submission License Agreement | 选择表示**同意/授予**的那个选项（通常形如 *I/we agree…*），**按页面实际选项文字选，不要凭印象** | 不同意则无法提交 |

> 这两个 consent 字段的确切含义以页面上的说明文字为准；如果文字与上表理解不符，以页面为准。

### 3.5 参会与签证（**现在就要填**）

这两个字段问的是**预计到场报告的那一位作者（presenting author）**，不是全体作者，**只能填一个人的情况**。国家要用**两字母 ISO 代码**，不要写国名全称。

| 情况 | Visa Needs | Country Of Origin |
|---|---|---|
| 由第一作者到场报告（中国护照，赴希腊需申根签证） | `yes` | `CN` |
| 由老师到场报告（日本护照，短期入境免签） | `no` | 通常留空 |

**建议填 `yes` / `CN`**：预计由第一作者报告，且勾选需要签证不产生任何义务，只是让主办方在需要时能出邀请函；反之若填 no 而后来需要邀请函会很被动。

> ⚠️ 不能同时填 China 和 Japan，也不要写完整国名。
> 这两项**不构成参会承诺**。EACL 2027（雅典，2027-03-09–14）为 hybrid 形式，远程报告在计划之内。具体签证办理方式与时限**不以本文件为准**，等录用后按希腊驻日使领馆官方信息办理。


---

## 4. Responsible NLP Checklist

> ⚠️ **实际表单的编号与官网说明页不一致**（例如实际 A 段第一问可能是 Potential Risks 而非 Limitations；B 段可能把"发布权利""PII""冒犯内容"拆成独立小问）。
> **因此下面按"问题内容"归档，不写死编号。粘贴时先读表单上的问题文字，再找对应条目。**
> 表单只接受 **Yes / No / N/A**，没有"部分"。**如实的 No 是安全的；虚报的 Yes 才可能导致拒稿。**

### A 段（当前表单仅有 Potential Risks）

**A1 Potential Risks：是否讨论了潜在伦理、社会或环境风险？ → Yes**
> The unnumbered "Ethical considerations" section discusses systematic subgroup errors, the risk of treating detector outputs as evidence of authorship or misconduct, and cautions against high-stakes deployment without human review and domain-specific validation. Section 6 additionally examines the risk of reusing detector probabilities as generation-quality scores or rewards.

> 📌 当前表单要求 Yes/No 时必须填 elaboration；以上文字同时给出明确章节位置。Limitations 仍是论文格式上的强制章节，但不是当前 A1 的问题，不要把它粘到 Potential Risks 的回答中。

### B 段（使用/创建科学工件 → Yes，然后逐小问）

**问"是否引用了所用工件的创建者" → No**
> Section 3 and the References cite the creators of all five source corpora and of the pre-trained models and methods used (DeBERTa-v3, mBERT, XLM-RoBERTa, Qwen3, DeepSeek-R1, QLoRA). The software packages we rely on (PyTorch, transformers, NumPy, SciPy, scikit-learn, pandas, peft, datasets) are identified by name and version in Appendix B.3 but are not cited bibliographically, so we answer No.

> 📌 选择 No 的原因不是语料与模型没有引用，而是软件包只报告了名称和版本，未逐项提供创建者文献引用。No 配上述说明比笼统声称“全部工件均已引用”更准确。

> 🔑 实际表单中的 B2--B4 均以 **"If you are releasing…"** 为条件。本轮不上传代码或数据，因此三项固定答 N/A；不要再在 Yes/No 与 N/A 之间摇摆。

**B2：若发布基于既有工件的材料，相关许可或权利是否允许？ → N/A**
> We are not releasing data or code with this submission, so this item does not apply. Appendix A.1 nevertheless reports the licenses we identified for the source corpora and the scope we treat as redistributable.

**B3：若发布的工件可能包含个人可识别信息，是否讨论缓解措施？ → N/A**
> We are not releasing data with this submission, so this item does not apply.

**B4：若发布的工件可能包含冒犯性内容，是否讨论缓解措施？ → N/A**
> We are not releasing data with this submission, so this item does not apply.

**问"是否提供了工件文档" → Yes**
> Section 3 and Appendix A document the source corpora, sampling, labeling, generation templates, parameters and post-processing, the four-quadrant composition, and the mapping between dataset names.

**问"是否报告了数据统计量与划分" → Yes**
> Section 3 and Appendix A report SarcTrain (10,000; five-fold), AuthTrain (19,995 = 9,999 human + 9,996 AI, with a 90/10 held-out split), GenTrain (2,428) and DualTest (4,664, with quadrants of 823/1,549/779/1,513). The evaluation protocol is given in Section 4.2.

### C 段（计算实验 → Yes，然后逐小问）

**问"参数量 / 总算力预算 / 计算基础设施" → Yes**
> Appendix B.3 reports the encoder parameter counts (178M mBERT, 278M XLM-RoBERTa-base, 184M DeBERTa-v3-base, 435M DeBERTa-v3-large) and the ~8B Qwen3 generator, the single NVIDIA RTX 3090 (24GB) used for all detector training and inference, approximately 170 GPU-hours for the fold-based backbone, architecture and ablation experiments together with the final models, and about two hours for SFT. Local text generation through Ollama was not timed separately, and the representation analyses run on CPU; Appendix B.3 states these exclusions explicitly rather than folding an unmeasured estimate into the reported figure.

> 📌 **这段与论文一致，已核对**：附录 B.3 现行文本确实写着 "approximately 170 GPU-hours" 与排除范围（"excludes local text generation through Ollama, SFT (about two hours), and the representation analyses, which run on CPU"）。**"tens of GPU-hours" 是 08-02 修订前的旧文本**。
> 关于"排除了部分实验还答 Yes 是否矛盾"：我们报告的 170 GPU-hours 覆盖全部 fold 实验、消融与最终模型，并把未计时的部分显式列出。本地生成耗时当初未记录日志，不能事后编造估算；当前表单答案固定为 Yes，并用上段文字透明说明覆盖范围与排除项。

**问"实验设置 / 超参搜索 / 最终超参" → No（保守且符合当前可确认事实）**
> Sections 4.2 and Appendix B report the final optimization settings, validation protocol, early stopping, fold and seed settings, and the mode-plus-one refitting rule for the final models. Appendix B.2 states that no systematic hyperparameter search was conducted and that DualTest was not used to choose the optimization settings. However, we cannot reconstruct whether limited manual pilot adjustments occurred or report the outcomes of every such trial, so we answer No rather than claim complete documentation of all tuning experiments.

> 📌 **Appendix B.2 已采用同一保守口径**：
> `The optimization settings reported here and in B.1 were selected based on prior practice and hardware constraints. We did not conduct a systematic hyperparameter search, and DualTest was not used to choose these optimization settings.`
>
> 论文当前措辞只否定“系统性超参搜索”，不否定可能存在的有限手工调整。由于作者无法确认完整 pilot 历史，表单答 No 更诚实；No 配说明不等于实验无效，也不要求凭空补写不存在的搜索范围。

**问"是否报告了描述性统计" → Yes**
> Section 5, Section 6 and Appendices C.3–C.8 report means with standard deviations over five runs, Wilson 95% confidence intervals, McNemar tests, Wilcoxon signed-rank tests with Benjamini–Hochberg FDR correction, and Spearman rank correlations.

**问"是否说明了现成包的实现/模型/参数设置" → Yes**
> Appendices A.4, B.1–B.4, C.4 and E.2 report model identifiers, software versions, decoding settings, optimizer and training parameters, API evaluation settings, and probe solver parameters.

> 📌 章节定位已修正：LLM judge 的设置在 **C.4**（不在 B），probe solver 参数在 **E.2**，生成解码设置在 **A.4**，训练与软件版本在 **B.1–B.4**。已逐个核对小节标题存在。

### D 段（是否新招募人工标注者/被试 → No）

> No new human annotators or participants were recruited. Human-authored text and sarcasm labels come from existing public research corpora, the AI-side labels follow from the construction procedure, and the three "judges" in Section 6 are automated LLM API calls (Section 3, Section 6, Appendix C.4–C.5, "Ethical considerations").

**若选 No 后仍强制显示子项，D1–D5 全部选 N/A**，理由分别为：

- D1（指示/招募文本）N/A — 未招募。
- D2（报酬）N/A — 无付费。
- D3（知情同意/数据使用条款）N/A — 本文未重新取得同意；数据获取与参与者程序由源数据研究负责，本文未加以验证。
- D4（伦理审查）N/A — 无新的人类被试研究需送审。
- D5（被试人口统计）N/A — 本文不报告新的被试人口统计；语料的语言、体裁与标注来源见附录 A。

> ⚠️ 不要笼统声称"原始研究的知情同意、伦理审批或人口统计已由本文核实"——我们没有核实。

### E 段（AI 助手 → Yes）

> "Ethical considerations" discloses that AI-assisted tools were used for language editing, structural revision and formatting, and that they did not generate the reported experimental outputs or statistics. The research design, analysis and conclusions are the authors' own, and the authors take full responsibility for them.

---

## 5. 附件策略：本轮代码与数据均不上传

**决定与理由**

- **数据不传**：正文与附录从未承诺发布（附录 A.1 只界定"哪些属于可再分发范围"），不传不构成矛盾。
- **代码不传**：收益（一点可复现性印象分）与风险（匿名性一旦出事整轮作废）不对称；代码是**可选**附件，不传无任何合规问题。

**checklist 里怎么表述**

- 带"If you are releasing…"条件的小问一律 **N/A**（见第 4 节 B 段），因为本次提交确实没有发布任何工件。
- 🔴 **不写任何发布承诺**（作者决定）。checklist 里**不出现** "will be released upon acceptance" 之类的句子——那是对外的新承诺，不是零成本的补偿动作，作者未做此承诺。
- **正文无需任何改动**：已核查，`manuscript_eacl2027.tex` 与 `appendices_eacl2027.tex` 中 `will release / will be released / upon acceptance / plan to release` **零命中**，论文从未承诺发布。附录 A.1 现有的"可再分发范围"是条件性说明（"我们把哪些东西视为可再分发"），不是承诺，**保持原样**。
- **不要为此往正文加任何脚注**——正文卡在 p.8。

**代码包现状（本机已就绪）**

- 路径：`/home/yang/AC_Sarcasm/eacl2027/anonymous_code.zip`（同目录另有解包目录 `anonymous_code/`）
- 大小 148,664 字节 / 41 个文件；SHA-256 前 24 位 `a67c038cbf123d7ec091be79`
- 匿名化状态：绝对路径改为 `PROJECT_ROOT` 环境变量；项目名中性化为 `sarcasm-mgtd`；wandb project 名去内部标识；`.pyc` 与 `__pycache__` 全部排除（字节码会内嵌编译时绝对路径）；zip 用 `-X` 去除 UID/GID；`/home/`、姓名、邮箱、密钥模式扫描全部零命中
- ⚠️ 该文件**只存在于本机工作区**，不在任何共享位置；论文修改者在自己的环境里找不到属正常。

**若日后决定上传**

- ARR **允许直接上传匿名 `.zip` / `.tgz` 附件**，这是最简单的方式。
- 若改为提供在线仓库链接，用 **Anonymous GitHub**；**禁止** Google Drive / Dropbox 等会泄露账号的跟踪型链接。

---

## 6. 三条别踩的线

1. **审稿注册漏填 → 直接的 desk-reject 风险。** 提交完成后当天提醒老师。
2. **作者名单提交后冻结。** 提交前确认人数、顺序、profile。
3. **checklist 宁可如实答 No / N/A，不要虚报 Yes。** 当前建议：B1 与 C2 答 No 并说明；B2--B4 因本轮不发布工件答 N/A。不要把条件性问题误填成 Yes，也不要声称完整记录了无法重建的手工调参历史。

---

## 7. 提交当天操作顺序

1. 核对待上传 PDF 的 SHA-256；若与顶部当前快照不同，重新 XeLaTeX 全量构建并检查页限、字体嵌入、元数据、匿名性、Figure 3 名称和引用。
2. 登录 OpenReview，确认两人 profile 完整。
3. 新建 ARR 2026-08 提交 → 按第 3 节逐字段填 → 上传 PDF。
4. 按第 4 节填 Responsible NLP Checklist：**先读表单上的问题文字，再找本文件对应条目**，不要按编号硬套。
5. 提交，保存 forum 链接（commitment 与提醒老师都要用）。
6. **立刻**把作者控制台链接发给老师，提醒 08-05 EoD AoE 前完成审稿注册。
7. 自己也在同一截止前完成审稿注册。

---

**参考来源**（2026-08-02 复核）

- ARR CFP：https://aclrollingreview.org/cfp
- ARR 作者须知：https://aclrollingreview.org/authors
- Responsible NLP Checklist 说明页：https://aclrollingreview.org/responsibleNLPresearch （**注意：编号与实际表单可能不一致，以表单为准**）
- 审稿人须知：https://aclrollingreview.org/reviewerguidelines
- 豁免政策：https://aclrollingreview.org/exemptions2025
- ARR OpenReview 入口：https://openreview.net/group?id=aclweb.org/ACL/ARR
- OpenReview 注册 / 资料：https://openreview.net/signup ・ https://openreview.net/profile
- EACL 2027 征稿：https://2027.eacl.org/calls/papers/
- News Headlines 许可（2026-08-02 实测 JSON-LD = CC BY 4.0）：https://www.kaggle.com/datasets/rmisra/news-headlines-dataset-for-sarcasm-detection
