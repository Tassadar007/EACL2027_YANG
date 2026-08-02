# §5.4 表征诊断：实验逻辑链、可读性诊断与修改建议

> 生成于 2026-08-02，依据 `scripts/sft_eval/probing/` 全部 7 个脚本与 `results/sft_eval/probing_tier0/` 全部 5 个结果 JSON 逐一核对。
> 评估对象：当前 `manuscript_eacl2027.tex` 的 §5.4（499 词；原始版 597 词）。

---

## 第一部分：完整实验逻辑链（从代码还原）

整章回答**一个问题**：86.3% 的误判集中在 AI 文本上，那么讽刺判断是不是在"骑"文本来源的信息？论证按**强度递增分三层**，每层自带对照。

### 第 0 步：确认分析对象无误（`verify_probe.py`）

冻结模型上抓 1,024 维 pooled 向量 → 手算 logit 与模型输出最大误差 **1.4e-6**；用 pooled 复算 AI×非讽刺 FPR = **86.3%**。
→ 后续所有操作确实作用在产出主结果的同一对象上。

### 第 1 层：来源信息在表征里吗？（`tier0_probe.py`）

| 项 | 值 |
|---|---|
| 联合模型 source probe AUC（全集 5 折） | **0.986** |
| 单头（只训讽刺）模型 | **0.974** |
| 置换标签对照 | 0.512 |

**证明**：来源几乎完美可线性读出，且**不是联合训练造成的**——从未见过 MGTD 标签的单头模型也有 0.974，说明这是人类/生成文本本身的系统差异。
**不能证明**：讽刺头用了它。→ 口径必须是 **availability ≠ use**。

### 第 2 层：来源信号与讽刺决策有关联吗？（`tier0_probe.py` + `tier1_probe.py`）

**(a) 分数层**：AI×非讽刺子群上，MGTD 的 AI logit 与讽刺 logit 的 Spearman **ρ=0.519**（n=1513, p=3e-105）。
长度三分位分别 **0.44 / 0.56 / 0.63** —— 控制长度后不降反升，**排除长度混杂**。

**(b) 方向层**：

| 方向对 | 普通余弦 | 协方差加权余弦 |
|---|---|---|
| 讽刺头 vs MGTD 头 | −0.031 | **+0.487** |
| 讽刺头 vs source probe | −0.029 | **−0.412** |

普通余弦把 1024 个轴等权对待；表征实际只在少数高方差轴上变化。**按数据实际变化加权后，两个头的方向有实质重叠。**
代价：协方差用全集无标签向量 → transductive，只能算关联。

### 第 3 层：把来源删掉，误判会消失吗？（干预）

**协议**：DualTest 按四象限分层 50/50 → fit 半估计所有方向/变换 → 固定作用到 eval 半 → **讽刺头完全冻结**。

**两个读数**：
① **FPR gap** = AI×非讽刺 FPR − 人类×非讽刺 FPR，基线 **52.0 pp**（84.8 − 32.8）
② 变换后表征上**重新交叉验证**的 source AUC，基线 **0.982**

> ⚠️ **52.0 与正文前面的 67.0 是两回事**：67.0 是跨模型（联合 vs 单头），52.0 是同一联合模型内部两个来源组的差。

**待检验假设**：若决策骑在线性可得的来源信息上，删掉它 → gap 应缩小。

| 干预 | gap (pp) | 残余 AUC | 推翻/支持了什么 |
|---|---|---|---|
| 基线 | 52.0 | 0.982 | — |
| MGTD 头方向 | **56.0** ↑ | 0.982 | 单方向删除无效 |
| source probe 方向 | 51.9 | 0.981 | 单方向删除无效 |
| 欧氏随机方向（对照） | 52.5 | 0.982 | 证明"无效"不是方法失灵 |
| INLP k=16 | **72.1** ↑ | **0.974** | AUC 只掉 0.008 → **INLP 根本没删掉来源** |
| **LEACE** | **19.1** ↓ | **0.589** | 唯一同时压住两者 |
| 白化随机 ×10（对照） | 52.2（51.4–52.7） | 0.982 | **LEACE 效应是特异的** |
| **srcORTH-PC1** | **12.6** ↓↓ | **0.982（不动）** | **决定性反例** |

### 第 4 步：全章落点 —— 两个"看似成功"的结果各自被自己的对照打掉

**① LEACE 不是修复，是重新分配。**
gap 52.0 → 19.1，但两组**未加权平均 FPR 几乎不变（58.8 → 59.1）**，因为人类组 FPR 从 **32.8 涨到 49.5**。总错误没减少，只是换了承担者。

**② srcORTH-PC1 证伪了"gap 由残余来源决定"。**
若该推论成立，gap 变小必伴随 AUC 下降。但 srcORTH-PC1 把 gap 压得**更低**（12.6 < 19.1），source AUC **完全不动**（0.982）——该方向按构造取自来源子空间的**正交补**，probe 仍能从别处读出来源。
→ **gap 与残余来源可解码性可以脱钩。仅凭线性干预，无法把失效唯一归因于来源。**

### 全章结论边界

| 确立了 | 未确立 |
|---|---|
| 来源高度可线性获得（且非联合训练所致） | 来源是误判的原因 |
| 来源相关的分数与方向和受影响决策存在关联 | 存在唯一的因果性来源特征 |
| 固定线性干预能改变冻结头在两组间的错误分布 | 任何"缓解"或"修复" |

---

## 第二部分：当前版本为什么仍读不懂

当前版已修好三处结构问题（观察/干预二分、contrast direction 定义、52.0≠67.0 前置）。**但剩下的缺陷更致命：证据的数字被抽走，推理的"因此"被省略。**现在读起来像结论摘要，而不是可检验的论证链。

### 缺陷 1（最严重）：关键数字缺失，导致结论无法判别

> 现文："Removing either single source-related direction or up to 16 INLP directions does not narrow the gap."

- 实际是 **56.0 / 51.9 / 72.1**，其中两个是**变大**。"does not narrow" 掩盖了 INLP 让 gap 涨到 72.1 pp。
- 更严重：**没写 INLP 后残余 AUC 仍 0.974**。缺这个数，读者无法分辨"不缩小"是因为
  (A) 来源不重要，还是 (B) **根本没删掉来源**。
  **这是两个完全相反的结论**，而正确答案是 (B)。现稿让审稿人无从判断。

### 缺陷 2：对照实验被写没了

协议段只有 "Random directions and subspaces provide controls"，结果段只有 "covariance-matched random controls show no comparable change"，**均无数字**（52.5；52.2，范围 51.4–52.7）。
审稿人无法判断 LEACE 的特异性有多强——而这正是 LEACE 结果唯一的可信度来源。

### 缺陷 3：最重要的一句被写得无法理解

> 现文："…while leaving mean FPR across source groups nearly unchanged"

读者无从得知：什么的均值（两组未加权平均）、原值多少（58.8→59.1）、为什么这说明是重分配（人类组 32.8→49.5）。
**而且"重新分配"这个结论本身被删掉了**，只剩一个悬空的观察。这是全章第二重要的发现，现在等于没写。

### 缺陷 4：推理链的"因此"缺失

结果段是三句陈述 + 一句 Thus。每一步**推翻了什么假设**没有写出来：

| 步骤 | 应得出的结论 | 现稿有无 |
|---|---|---|
| 单方向/INLP 无效且 AUC 未降 | 这些算子根本没删掉来源 | ❌ |
| LEACE 有效 + 随机对照无效 | 效应特异于来源方向 | 半句，无数字 |
| LEACE 平均 FPR 不变 | 不是修复，是重分配 | ❌ |
| srcORTH-PC1 gap 更低但 AUC 不变 | gap 与来源可解码性脱钩 | 半句 |

### 缺陷 5：srcORTH-PC1 的论证价值没有说出来

> 现文："Thus, source can remain recoverable by a new classifier even when a removed component changes how the frozen sarcasm head distributes errors."

这是在**描述现象**，没说**为什么重要**——它证伪了"gap 由残余来源决定"这个推论，从而使 LEACE 的结果**不能**被解释成"来源导致误判"。审稿人读完不知道这一步的价值，会以为只是一个附带观察。

### 缺陷 6：缺少三层递进的框架句

开头是 decodable vs connected 的**二分**，而实际论证是**三层递进**（可得性 → 关联 → 干预）。读者不知道后面有几层、每层强度如何，也就无法评价"证明力是否足够"。

---

## 第三部分：修改建议（四处补丁，共 +92 词 → 591 词，仍短于原稿 597）

**不改结构、不动任何既有措辞与 hedge**，只把被抽走的证据和"因此"加回去。所有数字均已从 JSON 核对。

### 补丁 1｜开头补三层框架（+25 词）

**原文**
> We separate observational diagnostics from interventions on held-out representations.

**替换为**
> We proceed in three steps of increasing strength---whether source is present, whether it is associated with the sarcasm decision, and whether removing it changes that decision---separating observational diagnostics from interventions on held-out representations.

### 补丁 2｜结果段第一句补数字与残余 AUC（+27 词）**［最高优先级］**

**原文**
> Removing either single source-related direction or up to 16 INLP directions does not narrow the gap.

**替换为**
> Removing either single source-related direction or up to 16 INLP directions does not narrow the gap (56.0, 51.9, and 72.1 pp), and residual source AUC stays above 0.97, so these operators do not remove source in the first place.

### 补丁 3｜LEACE 句补对照数字与"重分配"（+35 词）**［最高优先级］**

**原文**
> LEACE changes the gap from 52.0 to 19.1 pp and source AUC from 0.982 to 0.589 while leaving mean FPR across source groups nearly unchanged; covariance-matched random controls show no comparable change.

**替换为**
> LEACE changes the gap from 52.0 to 19.1 pp and source AUC from 0.982 to 0.589, whereas ten covariance-matched random directions leave the gap at 51.4--52.7 pp, so the effect is specific to the source direction. The two groups' unweighted mean FPR is nevertheless almost unchanged (58.8 to 59.1), because human non-sarcastic FPR rises from 32.8 to 49.5: LEACE reallocates errors rather than reducing them.

### 补丁 4｜srcORTH-PC1 句点明证伪价值（+5 词）

**原文**
> Removing srcORTH-PC1 changes the gap to 12.6 pp while source AUC remains 0.982. Thus, source can remain recoverable by a new classifier even when a removed component changes how the frozen sarcasm head distributes errors.

**替换为**
> Removing srcORTH-PC1 narrows the gap further, to 12.6 pp, while source AUC remains 0.982. The gap and residual source decodability therefore move independently, so the LEACE result cannot be read as evidence that source alone drives the errors.

### 优先级

若版面紧张只能改两处，做**补丁 2 和补丁 3**——它们分别修复"结论无法判别"和"最重要发现等于没写"这两个最致命的问题。

### 不需要改的部分

- 观察/干预二分、contrast direction 定义、52.0≠67.0 澄清、三个 intervention families 的分类：**保留，这些是本版的改进**。
- 所有 hedge（availability≠use / transductive / association not causal / not a unique causal source feature）：**一字不动**。
- 附录 E 与 Table 17：数字完整，无需改动。
