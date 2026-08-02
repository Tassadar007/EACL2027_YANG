# 技术评审回应草稿（与 2026-08-02 投稿文稿同步）

感谢评审对标签构造、训练对照、语言分层和实现定义的仔细检查。我们依据原始生成记录、逐实例预测和分析代码复核了全部关键问题。当前修订进一步明确了 AI 侧标签的操作性定义、完整联合训练流程的比较对象、跨语言与跨生成器结果，以及表征分析中 full-set 与 split-sample 协议的区别。核心双--单头效应及双语结论不变；新增说明收紧了证据边界，不改变实验数字或贡献定位。

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

Plain 的两种语言 exact McNemar 检验均为 $p<.001$；RCNN 的 English 检验同样为 $p<.001$，Chinese 为 $p=2.4\times10^{-5}$。因此聚合效应不由单一语言产生，当前 bilingual diagnosis 的定位有直接分层证据支持。语言仍与语料和体裁混杂，故论文不把这些差异解释为纯语言效应。

## 4. 数据划分与训练—测试独立性

复核确认，SarcTrain 的过采样发生在五折分配之前，因此部分重复记录会同时出现在一次训练--验证划分的两侧；这会削弱检查点选择的独立性，但相关记录不进入 DualTest。修正后的记录级匹配确认，SarcTrain 与 AuthTrain 均未和 DualTest 的人类侧或 AI 侧共享记录，最终复核未发现训练--测试泄漏。

表征干预使用另一个 50/50 子群分层切分。该切分不以 human--AI seed pair 为分组单位，24.7% 的保留 pair 跨越拟合半与评估半。论文已在附录 E.1 与 Limitations 中单独披露；这一问题不同于训练数据的跨折重复。

## 5. Qwen3.5 与 GPT-5-nano 样本量

Qwen3.5 生成的 AI 测试子集没有生成失败，其中 non-sarcastic 条件保留 1,550 条，而 DeepSeek 对应条件保留 1,513 条。生成器特定 FPR 因此使用各自的全部保留输出，而不是相同 seed intersection。当前文稿不再声称 Qwen3.5 子集由 2,373 个完整 human--AI pair 构成。

附录 C.4 比较 base 与 SFT。唯一 English 缺失发生在 GPT-5-nano 的 base 条件，故对应配对检验为 $n=119$；三条 Chinese 缺失发生在人类 reference 条件，不影响 C.4 的 Chinese base--SFT 行，只使 C.5 的 SFT--reference 比较变为 $n=177$。四个实例均仍有两个有效 judge ratings，因此使用 available-rating mean 时，聚合相关仍保留全部 600 个 base/SFT 输出。

## 6. 学习率与架构比较

核心讽刺双--单头比较的 sarcasm-head learning rate 均为 $5\times10^{-6}$。MGTD 头和部分 RCNN 中间参数的学习率并非完全匹配；修订后的附录 B.1 将各配置分行列出，并将跨架构结果限定为配置级描述，而非模块的独立因果效应。

## 7. 表征定义与干预解释

我们按实际代码澄清：$\rho=0.519$ 是 MGTD-head AI logit 与 sarcasm-head logit 在 AI $\times$ non-sarcastic 子集上的相关，而不是 probe direction 与标量的相关。协方差加权对齐使用全部 DualTest pooled 表征上的 Ledoit--Wolf 协方差，并计算 $\cos(\Sigma^{1/2}w_1,\Sigma^{1/2}w_2)$；绝对对齐值 0.41 和 0.49 分别对应 source-probe 与 MGTD-head 方向相对 sarcasm-head 方向的结果。由于这里使用全部未标注 DualTest 表征的协方差，该测量是 transductive association，而不是因果证据。

部分 Euclidean removals 还使用全部未标注 DualTest 表征的 pooled mean；LEACE、白化随机控制和 srcORTH-PC1 则只使用拟合半统计量。58.8%→59.1% 是 LEACE 下两个非讽刺来源组 FPR 的未加权均值，说明其主要作用是重新分配错误，而不是降低平均错误。srcORTH-PC1 改变 subgroup gap、但不降低 residual linear source AUC；这些结果显示冻结决策对表征几何的敏感性，没有识别唯一的来源因果子空间或通用缓解方法。

## 8. Detector--judge 分析

论文已经把 pooled $\rho=-0.212$ 标记为受 base/SFT 条件反转驱动的描述量，并报告条件内相关。主要条件比较采用按 prompt 配对的 Wilcoxon signed-rank test 和 BH-FDR 校正。因此本文结论是 detector probabilities 需要外部验证，未声称存在稳定的逐实例排序关系或人类感知失配。

## 9. 版本与复现范围

论文报告实际模型名、Ollama tag、生成参数、调用月份和软件环境。历史 DeepSeek digest 已无法复核，因此我们不提供无法验证的标识，也不声称 bitwise reproduction。DeepSeek-V4-Flash 使用 2026 年 4 月 24 日的官方 API announcement；GPT-5 使用 OpenAI 官方 System Card，参考文献年份为 2025；Gemini-2.5-Flash-Lite 与 Qwen3.5-9B 分别引用官方 model card。