# 技术评审回应草稿

感谢评审对标签构造、训练对照、语言分层和实现定义的仔细检查。我们依据原始生成记录、逐实例预测和分析代码复核了全部关键问题。复核确认核心双--单头效应及双语结论不变，同时发现三处需要澄清的技术表述：$
ho=0.519$ 对应两个任务头的 logit，白化几何需补充实际协方差定义，58.8%→59.1% 仅对应 LEACE。修订只校正这些定义，不改变实验结果或贡献定位。

## 1. AI 侧标签与类条件负迁移

我们同意 AI 侧标签记录 prompted condition，而非独立标注的 perceived sarcasm；正文数据定义和 Limitations 已明确这一边界。本文的结论是相对于同一操作性任务目标的负迁移：双头和单头模型在同一文本、同一参考标签上比较，五次配对运行的讽刺 F1 均下降，固定模型的目标子群预测差异也高度显著。我们不把 19.3% 解释为标签噪声上界，也不把 86.3% 外推为经人工裁定的感知讽刺错误率。独立实现后标注会扩展结论范围，但不否定当前任务定义下的训练流程效应。

## 2. 双头--单头对照的因果对象

现有对照识别的是完整联合训练流程，而不是单独的 MGTD 梯度。论文已经披露单头和双头在任务调度、共享更新量和早停信号上的差异，并在 Limitations 中明确称其为 complete joint-training procedure effect。因而“由联合训练诱导”仍由同架构、同讽刺数据、同文本配对比较支持；compute-matched no-MGTD control 属于进一步分解机制的实验，而不是当前行为结论成立的前提。

## 3. 核心效应并非由单一语言驱动

逐实例预测的语言分层复核显示，两种语言中的双--单头差距均同向且显著：

| 对比 | English | Chinese |
|---|---:|---:|
| Plain dual -- single | 84.1% -- 20.6% = +63.5 pp | 94.1% -- 14.7% = +79.4 pp |
| RCNN dual -- single | 37.9% -- 21.0% = +16.9 pp | 20.9% -- 13.3% = +7.7 pp |

Plain 的两种语言 McNemar 检验均接近 $p=0$；RCNN 的 English 检验同样接近 $p=0$，Chinese 为 $p=5.0\times10^{-5}$。因此聚合效应不由单一语言产生，当前 bilingual diagnosis 的定位有直接分层证据支持。语言仍与语料和体裁混杂，故论文不把这些差异解释为纯语言效应。

## 4. 数据划分与残余完全匹配

复核确认过采样副本和部分 human--AI pair 可跨训练/验证部分；这会影响早停信号的独立性，但不把相关记录带入独立 DualTest，也不改变报告指标的计算。报告中的七条训练记录实际对应三个唯一测试文本；从测试集删除这三条后，各核心 AI $\times$ non-sarcastic 比率变化不超过 0.05 pp。该敏感性说明它们对报告效应量的直接贡献可忽略，但我们不将删除测试解释为消除了训练侧影响。

## 5. 1,550 与 GPT-5-nano 样本量均正确

Qwen3.5 复现集由 2,373 个完整 human--AI pair 构成，其中 non-sarcastic 条件为 English 1,200 + Chinese 350 = 1,550。它与主生成器的保留集不是相同 seed intersection；附录现有说明准确，因此不改为 1,549。

C.3 比较 base 与 SFT。唯一 English 缺失发生在 base，故 English $n=119$；三条 Chinese 缺失均发生在人类 reference 条件，不影响 C.3 的 Chinese $n=180$，只使 C.4 的 SFT--reference 比较变为 $n=177$。三评审均分使用 pandas 的 available-rating mean，四个实例均仍有两个有效评分，因此总体 $n=600$ 正确。修订仅澄清缺失所在条件，所有表中数字保持不变。

## 6. 学习率与架构比较

核心讽刺双--单头比较的 sarcasm-head learning rate 均为 $5\times10^{-6}$。MGTD 头和部分 RCNN 中间参数的学习率并非完全匹配，附录 B.1 已明确披露；Limitations 也已将跨架构结果限定为配置级描述，而非模块的独立因果效应。因此无需重写学习率表或撤销配置比较。

## 7. 表征定义与干预解释

我们采纳表述精度问题并按实际代码澄清：$
ho=0.519$ 是 MGTD-head AI logit 与 sarcasm-head logit 在 AI $\times$ non-sarcastic 子集上的相关，而不是 probe direction 与标量的相关。白化对齐使用全部 DualTest pooled 表征上的 Ledoit--Wolf 协方差，并计算 $\cos(\Sigma^{1/2}w_1,\Sigma^{1/2}w_2)$；0.41 和 0.49 分别对应 probe 与 MGTD-head 来源方向。

58.8%→59.1% 是 LEACE 下两个非讽刺来源象限的未加权均值。修订将其明确限定为 LEACE。该结果与正交高方差控制共同支持论文现有解释：干预改变来源组间错误分配，但没有识别唯一的来源因果子空间。

## 8. Detector--judge 分析

论文已经把 pooled $\rho=-0.212$ 标记为受 base/SFT 条件反转驱动的描述量，并报告条件内相关。主要条件比较采用按 prompt 配对的 Wilcoxon signed-rank test 和 BH-FDR 校正。因此本文结论是 detector probabilities 需要外部验证，未声称存在稳定的逐实例排序关系或人类感知失配。

## 9. 版本与复现范围

论文报告实际模型名、Ollama tag、生成参数、调用月份和软件环境。历史 DeepSeek digest 已无法复核，因此我们不提供无法验证的标识，也不声称 bitwise reproduction。GPT-5 System Card 引用采用 arXiv:2601.03267，年份保留为 2026。