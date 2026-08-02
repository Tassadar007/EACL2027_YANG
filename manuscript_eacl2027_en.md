# Joint Training with Machine-Generated Text Detection Can Distort Sarcasm Detection: Diagnosing Class-Conditional Negative Transfer

## Abstract <!-- budget: ~0.35 pages -->

Sarcasm detection requires inferring intent beyond literal wording. LLM generation can expand sarcasm resources, but evaluation must distinguish sarcasm-relevant pragmatic cues from source-related ones. A shared encoder supports both sarcasm detection and machine-generated text detection (MGTD), yet source-predictive cues may distort sarcasm predictions. We harmonize Chinese and English resources and construct DualTest for source × sarcasm subgroup analysis; for AI-generated texts, sarcasm labels encode the prompt-specified generation style. On AI-generated texts prompted to be non-sarcastic, the baseline shared-encoder joint procedure yields a false-positive rate 67.0 percentage points higher than that of the corresponding sarcasm-only procedure, a pattern we call *class-conditional negative transfer*. Within AI-generated texts, sarcasm AUC remains similar across models, showing preserved ranking, while score distributions reveal an upward shift. The higher false-positive rates appear in both languages and persist when the same detectors evaluate outputs from another generator without retraining. Representation diagnostics link source-related features to the affected decisions and show that removing one data-derived high-variance direction changes the subgroup gap. Exploratory analysis shows predicted human-authorship probabilities do not consistently track LLM-judge ratings of human-likeness obtained without ground-truth source labels. These findings motivate source × sarcasm subgroup evaluation and external validation before detector probabilities are reused as quality scores or rewards.

## 1 Introduction <!-- budget: ~1 page -->

Sarcasm is a form of non-literal expression whose intended meaning may oppose its surface wording. It relies on implicit intent, contrast, context, and background knowledge, making automatic detection difficult. Datasets also differ in how sarcastic texts are elicited or collected and whose judgments define the labels; these choices, together with domain, style, and language, affect annotation and model behavior.

LLM-based data augmentation offers one potential response, but source-specific artifacts may become shortcuts. Evaluation must therefore separate source and sarcasm: human-authored and AI-generated texts may each be sarcastic or non-sarcastic. We refer to the four resulting combinations—human-authored sarcastic, human-authored non-sarcastic, AI-generated sarcastic, and AI-generated non-sarcastic texts—as source × sarcasm subgroups.

A shared encoder can reuse one representation for sarcasm detection and MGTD, but it also creates a route for interference: source-predictive features may influence the sarcasm head even though source should not determine sarcasm. Because these regularities vary across corpora and languages, we examine Chinese and English separately.

In this paper, we construct DualTest, a bilingual evaluation set for studying joint MGTD–sarcasm training with separate source and sarcasm labels across the four subgroups. For AI-generated texts, sarcasm labels record prompt-specified style. Using DualTest and same-configuration sarcasm-only controls, we show that joint training sharply increases sarcasm false positives on AI-generated texts prompted to be non-sarcastic. We call this pattern *class-conditional negative transfer* because the degradation is concentrated in one subgroup rather than distributed uniformly across DualTest. The direction persists on Qwen3.5-generated test texts, while two alternative representation configurations show lower observed rates than the plain configuration.

To connect encoded information with model behavior, we use linear probes to test whether source can be recovered, compare MGTD- and sarcasm-head scores, and remove selected representation directions before reapplying the unchanged sarcasm head. These diagnostics link source-related structure to the affected decisions and reveal sensitivity to a residual high-variance direction.

As a supplementary downstream analysis, we examine whether detector probabilities agree with separately obtained ratings of sarcasm and human-likeness. This matters because classifier scores may be reused as evaluation or reward signals for generated text. We therefore compare detector sarcasm and human-authorship probabilities with LLM-judge ratings obtained without ground-truth source labels.

**Contributions.** Following the structure of the paper, our contributions are fourfold:

- **Bilingual resources and controlled comparisons (§3–§4).** Four bilingual datasets: SarcTrain (10,000 sarcasm-labeled human texts), AuthTrain (19,995; 9,996 retained AI counterparts for 9,999 human seeds), GenTrain (2,428 generation instructions), and DualTest (4,664), whose instances carry both `is_sarcastic` and `is_human` labels and support analysis over the four source × sarcasm groups. To the best of our knowledge, DualTest is the first dual-label stratified test set for sarcasm evaluation, and the dual-head model built on it is the first to formulate the two tasks as a joint multi-task problem (§2.2).
- **Subgroup-level evidence of class-conditional negative transfer (§5).** The plain joint model misclassifies 86.3% of AI-generated texts prompted to be non-sarcastic as sarcastic; a matched sarcasm-only model misclassifies only 19.3%. The increase is induced by the complete joint-training procedure, its direction persists on Qwen3.5-generated test texts, and two alternative representation configurations show lower observed rates.
- **Representation evidence and a target for causal analysis (§5.4).** Source information is highly decodable from the sarcasm representation and correlated with sarcasm decisions. The diagnostics reveal sensitivity to a residual high-variance direction estimated from the fitting split, motivating semantic characterization and targeted causal tests.
- **An exploratory comparison between detector probabilities and LLM-judge ratings (§6).** Predicted human-authorship probabilities and LLM-judge ratings of perceived human-likeness rank generated outputs differently, and sarcasm-score agreement varies by language. This downstream analysis shows why detector probabilities require external validation before use as generation-quality or reward signals.

## 2 Related Work <!-- budget: ~0.95 pages -->

### 2.1 Sarcasm Detection and Multilingual Resources

Feature-engineered methods (Riloff et al., 2013), neural sequence models (Ghosh & Veale, 2016; Tay et al., 2018), and pretrained language models (Khatri & P, 2020; Han et al., 2022) have each advanced sarcasm detection. The task remains difficult in multilingual and class-imbalanced settings, especially when labels target intended rather than perceived sarcasm. Although scores are not directly comparable across datasets, languages, modalities, or evaluation setups, they illustrate substantial benchmark variability: the top two English systems on iSarcasmEval (Abu Farha et al., 2022) report positive-class F1 of 0.605 and 0.569; zero-shot ChatGPT positive-class F1 ranges from 0.152 to 0.567 across MultiPICo languages (Casola et al., 2024); and a text-only XLM-R baseline reaches 0.734 F1 on the bilingual multimodal SarcNet benchmark (Yao et al., 2024). RCNNs combine recurrent context with convolutional pooling for text classification (Lai et al., 2015) and have also been evaluated for multilingual sarcasm detection (Yang & Ikeda, 2024); we therefore include an RCNN intermediate configuration in §4.1. Because these benchmark results come from different datasets, §5.1 provides directly comparable mBERT dual-head baselines on DualTest. We use a bilingual Chinese–English setting to test whether the subgroup failure is confined to one language, rather than treating cross-language transfer as the primary objective.

### 2.2 Machine-Generated Text Detection and Authorship Verification

Machine-generated text detection (MGTD) uses zero-shot/perplexity-based detectors (Mitchell et al., 2023) and discriminative models trained on paired human–AI corpora (HC3; Guo et al., 2023); generalization is the recognized central challenge, motivating M4 (Wang et al., 2024) for black-box detection across generators, domains, and languages and RAID (Dugan et al., 2024) for robust detector evaluation. Conceptually, Bevendorff et al. (2025) frame the problem as authorship verification rather than quality assessment; we treat MGTD as an auxiliary task within a shared encoder. Prior work treats sarcasm detection and MGTD as unrelated problems: existing multi-task sarcasm research mainly combines sarcasm with sentiment, stance, or emotion, and to the best of our knowledge no work has studied the two as a joint multi-task problem.[^search] How a shared encoder behaves when both judgments are learned jointly is therefore an open question and the starting point of this paper.

[^search]: The search was conducted in May 2026 and updated in July 2026 across Semantic Scholar, arXiv, ACL Anthology, and Google Scholar, using keyword combinations including “sarcasm detection,” “machine-generated text detection,” “AI-generated text detection,” “multi-task learning,” and “joint detection.”

### 2.3 Multi-Task Negative Transfer, Shortcut Learning, and Shared-Encoder Mitigation

We use negative transfer and shortcut learning as explanatory frameworks for a previously unstudied task combination. In multi-task learning, auxiliary tasks are not always beneficial: transfer direction depends on task relatedness and data geometry (Zhang et al., 2023; Wu, Zhang, & Ré, 2020), and conflicting gradients can cause negative transfer (Yu et al., 2020; Liu et al., 2021). Mueller et al. (2022) provide the closest NLP precedent, showing through single-/multi-task contrasts that models may infer task identity from the input distribution and destabilize on out-of-distribution inputs (see also He & Choi, 2021; Li et al., 2023, on internal mechanisms of multi-task models). Shortcut learning is complementary: models may exploit features predictive in the training distribution but failing on specific subgroups (Geirhos et al., 2020; Ye et al., 2024), which in multi-task settings appears as increased absorption of spurious correlations (Hu et al., 2022b; Chai & Wang, 2025) and subgroup-stratified failures (Compton et al., 2023). Shared-encoder mitigation either isolates task-specific parameters (Pfeiffer et al., 2020, 2021; Karimi Mahabadi et al., 2021; Stickland & Murray, 2019) or modifies information flow within shared computation (Wu et al., 2019; Pilault et al., 2021). Our analysis is closer to the second design direction: we compare a no-module baseline with two intermediate-representation variants and test whether degradation changes across source × sarcasm subgroups (§5.3).

### 2.4 Detector Outputs as Evaluation and Reward Signals

Beyond the main line of failure diagnosis, dual-head detector outputs might also be used as evaluation or reward signals for generated text; this section gives the background for the supplementary analysis in §6. LLM-as-a-judge (Zheng et al., 2023) is widely used for generation evaluation, and using automated scorers as rewards risks reward hacking in the Goodhart sense. The practice has entered sarcasm generation: ViSP (Wang et al., 2026) uses scores from the sarcasm detector DIP (Wen et al., 2023) as a PPO reward, motivating a direct comparison between detector sarcasm probabilities and independent sarcasm ratings. A parallel but distinct question concerns MGTD: verification probability is not itself a measure of perceived human-likeness. Section 6 compares both pairs of signals.

## 3 Dataset Construction <!-- budget: ~0.9 pages -->

We begin with five heterogeneous sources (iSarcasm: Oprea & Magdy, 2020 / Abu Farha et al., 2022; CSC: Jang & Frassinelli, 2024 / Jang et al., 2023; ToSarcasm: Liang et al., 2022; News Headlines v2: Misra & Arora, 2023 / Misra & Grover, 2021; Open Chinese Internet Sarcasm Corpus: Zhu, 2022; 44,862 instances after standardized merging, approximately 87% English) and obtain three training sets and one diagnostic test set through a three-stage pipeline. Source-level details, annotation methods, and quality checks are provided in Appendix A.

**Sarcasm classification training set, SarcTrain (10,000, all human).** The dataset is balanced across four language × sarcasm strata, with 2,500 instances per stratum. Three strata are downsampled; the Chinese sarcastic stratum starts from 2,396 records representing 2,394 unique texts and adds 104 oversampled records. The resulting set is organized into five folds.

**MGTD training set, AuthTrain (19,995).** The human side draws 9,999 approximately language-balanced seeds from SarcTrain (5,000 English / 4,999 Chinese) after one Chinese seed is excluded during seed-level generation quality control. On the AI side, DeepSeek-R1-0528-Qwen3-8B (Ollama tag `deepseek-r1:8b`) is instructed to produce a topically related output from each selected seed. A total of 9,996 AI counterparts are retained; three selected human seeds remain unmatched, giving 99.97% pairing coverage. Four templates (Chinese/English × sarcastic/normal) follow the balanced seed labels. Authorship and requested-style labels are constructed as separate, approximately balanced factors; no independence is assumed for realized textual features or perceived sarcasm. Generation parameters, full templates, and post-processing steps are given in Appendix A.

**Generation training set, GenTrain (2,428; used in §6).** From 5,257 eligible positive-sarcasm records in iSarcasm and ToSarcasm, seed-42 sampling selects 2,103 records (1,214 English; 889 Chinese). Chinese oversampling adds 325 records, yielding 1,214 per language. By prompt type, the set contains 374 rewriting, 839 response, and 1,215 comment instances and serves as the instruction-tuning data for SFT.

**Diagnostic test set, DualTest (4,664).** This is the core data contribution of the paper: each instance bears both `is_sarcastic` and `is_human` labels. The human-authored subset comprises the official iSarcasm test split (1,400 English instances) and the ToSarcasm test set (972 Chinese instances). Generation and quality control retain deepseek-r1:8b counterparts for 2,292 of the 2,372 human-authored texts (96.6% coverage). Human-authored texts retain their annotations; for AI-generated texts, `is_sarcastic` records the prompt-specified style rather than independent human annotation of perceived sarcasm, and `is_human` is false by construction. After combining languages, the four quadrants contain 823 human × sarcastic, 1,549 human × non-sarcastic, 779 AI × sarcastic, and 1,513 AI × non-sarcastic instances (see Appendix A for the language × source × sarcasm distribution).

## 4 Dual-Head Detection Model <!-- budget: ~0.9 pages -->

### 4.1 Tasks and Architecture

Given a Chinese or English input text $x$, the model produces two separate, non-mutually-exclusive binary predictions: whether $x$ is labeled sarcastic and whether its authorship source is labeled human or AI. Any combination of the two labels is valid. The tasks share a single DeBERTa-v3-large encoder (He et al., 2023). We compare three representation configurations: (1) plain, which sends `[CLS]` directly to both heads; (2) interaction-projection, which adds residual projections between task-specific branches; and (3) RCNN, which applies recurrent and convolutional pooling before the heads.

**Plain.** The plain configuration is the shared-encoder baseline and sends `[CLS]` directly to both task heads.

**Interaction-projection.** This configuration forms learned 256-dimensional sarcasm and authorship (MGTD) branch vectors $h_s$ and $h_a$, and updates them as $h_s'=h_s+P_{a\rightarrow s}(h_a)$ and $h_a'=h_a+P_{s\rightarrow a}(h_s)$. Subscripts identify the branches, arrows give the mapping direction, and primes mark the updated vectors. Each branch contains one vector, so no query–key interaction remains; $P$ is the effective learned value/output projection.

**RCNN.** The RCNN configuration uses a BiLSTM followed by parallel one-dimensional convolutions and global max pooling. A single-layer bidirectional LSTM with hidden size 256 per direction produces a 512-dimensional output at each token position. We concatenate it with the 1,024-dimensional encoder output, then apply convolution kernels of widths 3/4/5 with 128 channels each. Global max pooling produces a 384-dimensional vector $z$, which passes through dropout to the two heads. The configurations retain the same encoder backbone and task definitions; remaining dimensions and dropout settings are in Appendix B.

![Figure 1](figures/fig_architecture.png)

**Figure 1: RCNN dual-head configuration: a shared DeBERTa-v3-large encoder, an RCNN intermediate module (dashed), and two task-specific heads. The comparisons in §5 remove this component in the plain configuration or replace it with two learned residual projections between the task branches, while retaining the same encoder backbone and task definitions (Appendix B).**

### 4.2 Training Protocol

We distinguish representation configuration (plain, interaction-projection, or RCNN) from training procedure (joint, sarcasm-only, or MGTD-only): joint models have two task heads, whereas task-specific controls have one active head. The protocol supports controlled joint–sarcasm-only comparisons in the plain and RCNN configurations. Within each comparison, the models use the same encoder architecture, intermediate configuration, sarcasm data and fold assignments, and sarcasm-specific hyperparameters. The joint model additionally receives AuthTrain batches, updates shared parameters from both tasks, follows the mixed-task schedule, and uses joint early stopping; the comparison therefore evaluates the complete joint-training procedure rather than any single component.

Training uses a mixed loader that alternates batches by task: each batch contains samples from only one task, and MGTD and sarcasm batches alternate in a 2:1 ratio, matching the data-size ratio between AuthTrain and SarcTrain. Let the shared parameters be $\theta_{\mathrm{sh}}$ (the encoder and intermediate module), let $z(x)$ be their aggregated representation for input $x$, and let $W_s$ and $W_a$ be the sarcasm and authorship (MGTD) classifiers. At step $t$, $B_t$ is the current task-homogeneous batch of input--label pairs $(x,y)$, and the loss is computed only for its task:

$$
\mathcal{L}(B_t)=
\begin{cases}
\dfrac{1}{|B_t|}\displaystyle\sum_{(x,y)\in B_t}\mathrm{CE}_{w}\bigl(\mathrm{softmax}(W_s\,z(x)),\,y\bigr), & B_t\subset \text{SarcTrain},\\[8pt]
\dfrac{1}{|B_t|}\displaystyle\sum_{(x,y)\in B_t}\mathrm{CE}\bigl(\mathrm{softmax}(W_a\,z(x)),\,y\bigr), & B_t\subset \text{AuthTrain},
\end{cases}
$$

where $\mathrm{CE}_w$ is sarcasm cross-entropy with class-weight vector $w$ (weights are provided in Appendix B). Although SarcTrain is class-balanced, sarcasm detection is the primary task and the sarcastic class is its focal positive class; the weighted loss therefore penalizes errors on sarcastic examples more heavily. The gradient flow follows directly from the equation: on sarcasm batches, $\partial\mathcal{L}/\partial W_a = 0$ (symmetrically, $\partial\mathcal{L}/\partial W_s = 0$ on MGTD batches), whereas $\theta_{\mathrm{sh}}$ receives gradients from both types of batches. The shared encoder and intermediate module are shaped jointly by both tasks, while each classification head is updated only by data from its own task.

Optimization uses separate learning rates for parameter groups: the pretrained encoder is updated at a much lower rate than the intermediate module and classification heads to preserve pretrained representations. Model selection accounts for both tasks. The early-stopping signal at epoch $e$ is the equally weighted sum of each task's validation loss normalized by its first-epoch value:

$$
\mathcal{L}_{\mathrm{stop}}^{(e)}=\tfrac{1}{2}\,\frac{\mathcal{L}_{s}^{(e)}}{\mathcal{L}_{s}^{(1)}}+\tfrac{1}{2}\,\frac{\mathcal{L}_{a}^{(e)}}{\mathcal{L}_{a}^{(1)}},
$$

where $\mathcal{L}_{s}^{(e)}$ and $\mathcal{L}_{a}^{(e)}$ are the epoch-$e$ validation losses for the sarcasm and MGTD tasks on their respective validation sets. Normalization prevents early stopping from being dominated by the larger MGTD task.

We use five fold-matched runs. In run $k$, four SarcTrain folds train the sarcasm task and the fifth provides validation; all runs share a fixed AuthTrain 90/10 split, whose approximately 2,000-example holdout provides a common MGTD validation set. Each run reloads its configuration's pretrained encoder and newly initializes the remaining modules and heads; all configurations use the same folds and seed schedule. DualTest is held out until evaluation. We report mean ± standard deviation across runs to summarize variation from the sarcasm fold and initialization; for subgroup rates from separately trained final models, Wilson intervals summarize uncertainty over DualTest examples (Table 3 and Appendix C.3).

For each configuration used in the subgroup analysis, we train a final model on all applicable training data with the same hyperparameters and a fixed epoch count (the mode of the five runs' best early-stopping epochs + 1), using an independent seed and no early stopping. These models provide the subgroup and representation analyses but are excluded from the five-run aggregates; the exploratory analysis in §6 uses the final RCNN dual-head model. Class weights, sampling ratio, grouped learning rates, and full hyperparameters are provided in Appendix B.

## 5 Experiments and Diagnosis <!-- budget: ~2.9 pages -->

We organize the analysis in four stages: system comparison, matched joint–single-task comparison, source × sarcasm evaluation across languages and generators, and representation-level diagnosis.

### 5.1 System-Level Comparisons

**Backbone comparison.** Under the same protocol, we compare dual-head mBERT (Devlin et al., 2019), XLM-RoBERTa-base (Conneau et al., 2020), and DeBERTa-v3 base and large (He et al., 2023); for DeBERTa-v3-large, we also test interaction-projection and RCNN pathways. DualTest positive-class F1 is reported for both tasks.

**Table 1: Descriptive dual-head results across backbones and architectural variants (DualTest; five-run mean ± standard deviation; bold = highest mean)**

| Model | Sarcasm F1 | MGTD F1 |
|------|---------|-----------|
| mBERT dual-head | 0.4945 ± 0.036 | 0.8743 ± 0.006 |
| XLM-RoBERTa-base dual-head | 0.4603 ± 0.057 | 0.8321 ± 0.025 |
| DeBERTa-v3-base dual-head | 0.5646 ± 0.056 | 0.8952 ± 0.003 |
| DeBERTa-v3-large plain dual-head | 0.6126 ± 0.031 | 0.9221 ± 0.010 |
| DeBERTa-v3-large + task-branch interaction projection dual-head | 0.6076 ± 0.020 | 0.9126 ± 0.007 |
| **DeBERTa-v3-large + RCNN dual-head** | **0.6309 ± 0.020** | **0.9301 ± 0.005** |

On DualTest, the DeBERTa configurations have higher mean F1 than mBERT and XLM-R, with RCNN highest on both tasks (0.6309 sarcasm; 0.9301 MGTD). Subsequent analyses focus on the DeBERTa-v3-large configurations, with the plain encoder as the reference and interaction-projection and RCNN as alternatives. Complete results are in Appendix C.

**Joint versus single-task training.** To isolate the second task, we compare plain and RCNN joint procedures with corresponding single-task procedures. Each pair fixes architecture and task-specific data; joint training additionally updates the shared encoder with the other task.

**Table 2: Joint vs. corresponding single-task procedures (mean ± standard deviation over five fold-based runs)**

| Configuration | Task | Joint | Single-task | Δ (joint − single-task) |
|------|------|------|------|------------------|
| Plain | Sarcasm | 0.6126 ± 0.031 | 0.6734 ± 0.010 | **−6.08 pp** |
| Plain | MGTD | 0.9221 ± 0.010 | 0.9054 ± 0.027 | +1.67 pp |
| RCNN | Sarcasm | 0.6309 ± 0.020 | 0.6751 ± 0.022 | **−4.43 pp** |
| RCNN | MGTD | 0.9301 ± 0.005 | 0.9253 ± 0.006 | +0.49 pp |

Sarcasm F1 is lower under joint training in all five matched runs, by 6.08 pp for plain and 4.43 pp for RCNN on average; MGTD F1 is higher by 1.67 and 0.49 pp. Joint training therefore produces a consistent aggregate sarcasm loss, smaller under RCNN.

### 5.2 Class-Conditional Negative Transfer

To locate the aggregate loss, we evaluate four source × sarcasm groups. Sarcastic groups use recall and non-sarcastic groups use false-positive rate (FPR); human texts retain dataset annotations, while sarcasm labels for AI-generated texts record prompt-specified style. Plain and RCNN provide matched joint–sarcasm-only comparisons, and interaction-projection provides an additional joint pathway.

**Table 3: Comparison of five sarcasm classifiers on four source × sarcasm quadrants (recall for sarcastic subsets; false-positive rate for non-sarcastic subsets; human labels are annotations and AI labels are prompted conditions; bold marks the AI non-sarcastic subgroup analyzed in the text)**

| Model (sarcasm prediction) | Human × sarcastic recall | Human × non-sarcastic false positive | AI × sarcastic recall | **AI × non-sarcastic false positive** |
|------------|----------------|------------------|--------------|--------------------|
| Plain dual-head | 67.1% | 32.9% | 98.2% | **86.3%** |
| Interaction-projection dual-head | 71.9% | 37.6% | 94.4% | **40.9%** |
| RCNN dual-head | 80.8% | 40.5% | 89.3% | **34.1%** |
| Plain single-head (sarcasm only) | 67.8% | 28.7% | 76.6% | **19.3%** |
| RCNN single-head (sarcasm only) | 66.2% | 28.5% | 79.1% | **19.3%** |

**Loss concentration.** On AI-generated texts prompted to be non-sarcastic, FPR rises from 19.3% under sarcasm-only training to 86.3% under plain joint training (67.0 pp); the matched RCNN increase is 14.8 pp. The plain-model increase on human-authored non-sarcastic texts is only 4.3 pp. The loss is therefore concentrated in the AI-generated non-sarcastic group, a pattern we call class-conditional negative transfer.

**Ranking and score shift.** Within AI-generated texts, the area under the receiver operating characteristic curve (AUC) for sarcasm remains 0.82–0.88. Treating prompted-sarcastic texts as positives, AUC can be interpreted as the probability that a randomly selected prompted-sarcastic text receives a higher predicted-sarcasm score than a randomly selected prompted-non-sarcastic text, with ties counted as one half. In the AI-generated non-sarcastic subgroup, the plain model's median predicted sarcasm probability rises from 0.204 under sarcasm-only training to 0.767 under joint training, and the joint model's interquartile range lies above the decision threshold. The high FPR therefore reflects an upward score shift rather than collapsed within-AI ranking.

### 5.3 Cross-Language and Cross-Generator Recurrence

The subgroup effect has the same direction in both languages. Joint–sarcasm-only FPR increases for English and Chinese are 63.5 and 79.4 pp for plain, and 16.9 and 7.7 pp for RCNN.

For generator transfer, unchanged detectors evaluate Qwen3.5-9B outputs generated with the same language × style templates. Plain joint and sarcasm-only FPRs are 91.3% and 7.9% (83.4 pp, versus 67.0 pp on DeepSeek), while matched RCNN rates are 22.1% and 10.6%. The higher joint-training FPR therefore recurs without detector retraining.

### 5.4 Representation-Level Diagnosis

For each DualTest text, the frozen final plain joint model produces a 1,024-dimensional pooled representation $h$. All operations below act on $h$. Interventions modify it before the unchanged sarcasm head is reapplied; the encoder and head remain frozen. Three steps test source availability, association, and intervention sensitivity. The matched sarcasm-only model is used only in Step 1.

**Step 1: is source available?** A linear probe tests whether an attribute can be recovered from frozen representations without changing the model (Hewitt and Liang, 2019). Its five-fold source AUC—0.5 denotes chance ranking and 1.0 perfect ranking—is 0.986 for joint-model representations and 0.974 for sarcasm-only representations. The answer is therefore yes: human-versus-AI source is highly linearly readable from both representation spaces, including that of the model trained without MGTD labels.

**Step 2: is source associated with sarcasm decisions?** A logit is a pre-softmax class score. On AI-generated non-sarcastic texts, MGTD AI and sarcasm logits have positive Spearman correlation ($\rho=0.519$): texts assigned higher AI logits also tend to receive higher sarcasm logits. Correlations within length terciles are 0.44, 0.56, and 0.63. A *direction* is a vector in the space of $h$. For a linear head, its class-weight difference is a contrast direction along which one logit increases relative to the other. Thus, $d_{\mathrm{sarc}}=W_s[1]-W_s[0]$ increases the sarcastic-over-non-sarcastic logit, while $d_{\mathrm{AI}}=W_a[0]-W_a[1]$ increases the AI-over-human logit. The source-probe direction is instead the normalized coefficient of a separately fitted logistic-regression probe.

Ordinary cosine weights dimensions equally and finds the head directions nearly orthogonal ($\cos\approx-0.03$). Covariance-aware alignment instead gives more weight to dimensions along which the observed representations vary more (Ledoit and Wolf, 2004); absolute alignment with the sarcasm direction is 0.49 for the MGTD-head direction and 0.41 for the source-probe direction. Thus, source-related scores co-vary with sarcasm scores, and covariance-aware alignment for the task heads is markedly larger in magnitude than ordinary cosine.

**Step 3: do interventions change sarcasm decisions?** DualTest is split 50/50 within each source × sarcasm subgroup. Label-dependent directions and transforms are estimated on the fitting half, fixed, and applied to the evaluation half. Direction removal centers $h$, subtracts its projection onto a selected direction or subspace, restores the mean, and passes the modified vector through the unchanged sarcasm head. These modified vectors constitute the transformed evaluation set.

The FPR gap—AI-generated minus human-authored FPR on non-sarcastic texts—measures how the frozen head allocates errors. Residual source AUC comes from a new cross-validated probe on the transformed representations; it is not MGTD-head performance. The untouched within-model baseline is a 52.0-pp gap with source AUC 0.982, distinct from the earlier 67.0-pp joint–sarcasm-only difference.

*Source-direction interventions.* To test whether a single learned source contrast controls the gap, we remove either the joint model's MGTD-head direction or the fitting-half source-probe direction. Their gaps are 56.0 and 51.9 pp, with source AUCs of 0.982 and 0.981. To target source information distributed across multiple linear directions, we apply INLP, which repeatedly fits a source classifier and removes its predictive direction to construct a multidimensional source-targeted subspace (Ravfogel et al., 2020). Removing 16 INLP directions gives a 72.1-pp gap and source AUC 0.974. These removals leave source highly readable and do not narrow the gap.

*Covariance-aware intervention.* We apply LEACE, a closed-form linear transform designed to reduce linearly decodable source information while accounting for representation covariance (Belrose et al., 2023). LEACE lowers source AUC from 0.982 to 0.589 and the gap from 52.0 to 19.1 pp, whereas ten covariance-matched random directions leave gaps of 51.4–52.7 pp. The two groups' unweighted mean FPR remains near 59% (58.8 to 59.1) because human non-sarcastic FPR rises from 32.8 to 49.5. LEACE therefore changes how the frozen sarcasm head allocates false positives between sources.

*Study-specific residual-geometry intervention.* To explore dominant geometry outside a low-dimensional source subspace, we use three INLP directions to define a study-specific residual space and extract its PC1 (*INLP-3 residual PC1*). On the fitting half, we (i) fit three orthonormal INLP source directions, (ii) project centered representations onto their orthogonal complement, (iii) run PCA on the residual representations, and (iv) retain the first, highest-variance component. On the evaluation half, we remove only this PC1 from each original representation; the three INLP directions define the search space but are not removed. This intervention narrows the gap to 12.6 pp while source AUC remains 0.982. The FPR gap therefore does not consistently track residual linear source AUC, so source decodability alone is insufficient to explain the frozen head's source-group error gap.

Step 3 therefore yields a selective result: LEACE and INLP-3 residual PC1 substantially narrow the frozen head's source-group FPR gap, whereas the tested head, probe, and INLP removals do not materially narrow it or substantially reduce residual source AUC. Across the three steps, source is available, source-related scores and data-weighted directions are associated with the affected decisions, and specific changes to representation geometry alter where false positives occur.

## 6 Exploratory Comparison of Detector Scores and LLM-Judge Ratings <!-- budget: ~0.4 pages -->

This section asks a related but separate question: whether a source-classification probability can serve as a proxy for perceived human-likeness, and whether the detector's sarcasm score tracks external LLM-judge ratings. These are comparisons between different constructs; a classification boundary need not measure style realization or generation quality. The failure in §5 may contribute to disagreement but is not treated as its sole cause.

We use Qwen3-8B (Qwen Team, 2025) as the base model and fine-tune it on GenTrain using QLoRA (Dettmers et al., 2023). The evaluation covers 180 Chinese topic-comment prompts and 120 English rewriting prompts. Base and SFT outputs are scored by the RCNN detector and by three LLM judges (DeepSeek-V4-Flash, Gemini-2.5-Flash-Lite, and GPT-5-nano) that are not shown the ground-truth source.

The detector probabilities and judge ratings do not move together consistently. In Chinese, SFT raises the RCNN detector's sarcasm score from 0.780 to 0.895 but lowers the mean LLM-judge rating of sarcasm from 3.99 to 3.80. The sarcasm correlation is nearly zero in Chinese ($-0.020$) but positive in English ($+0.338$). The clearest condition-level reversal concerns human-likeness: the MGTD head assigns higher predicted human-authorship probabilities to SFT outputs in both task–language settings, while mean perceived human-likeness ratings decline. Their pooled correlation is $-0.212$. Within-condition correlations are weaker, showing that the pooled negative value is driven mainly by the reversal between conditions.

**Table 4: Spearman correlations between detector probabilities and LLM-judge ratings (RCNN detector)**

| Subset | $n$ | Sarcasm $\rho$ | MGTD $\rho$ |
|------|---|------------|--------------|
| Overall | 600 | +0.136 (significant) | **−0.212** (significant) |
| Chinese | 360 | **−0.020** (not significant) | −0.218 (significant) |
| English | 240 | +0.338 (significant) | −0.270 (significant) |

The sarcasm-detector change is setting-dependent: +0.115 in Chinese and −0.124 in English. By contrast, all six language × judge comparisons give SFT lower mean sarcasm ratings, five significantly after FDR correction. Detector probabilities can therefore remain useful classification outputs while failing to preserve the ordering or condition-level direction required of style or quality scores. Judge agreement is low to moderate at the individual-text level, and a plain-detector control changes the reported correlations by at most 0.02. Because the judges are LLMs rather than human annotators, the analysis establishes disagreement between detector probabilities and LLM-judge ratings, not human-perceived quality, and it does not test reward optimization. The stable human-authorship/human-likeness reversal and judge-specific sarcasm results motivate external validation before detector probabilities are used as generation-quality scores or optimization rewards.

## 7 Conclusion <!-- budget: ~0.45 pages -->

LLM-based augmentation requires generated examples to convey sarcasm without turning source cues into shortcuts. Our results expose this risk: compared with the corresponding same-architecture sarcasm-only control, the baseline joint model predicts sarcasm more often for AI-generated texts prompted to be non-sarcastic, raising its false-positive rate by 67.0 pp. The direction holds across languages and on Qwen3.5-generated test texts, with the RCNN and interaction-projection configurations showing lower observed rates. Representation interventions show that source-related and other high-variance directions shape how errors are distributed across source groups, providing concrete targets for further causal analysis. Human-authorship probabilities diverge from perceived human-likeness, so detector scores require external validation across source × sarcasm subgroups before use as quality scores or rewards.

## Limitations <!-- mandatory; excluded from page limit; must not contain new results -->

The first set of limitations concerns data and experimental settings. Sarcasm labels for AI-generated texts in DualTest reflect prompt-specified style rather than independent human annotation, so an apparent false positive can reflect detector error or realized sarcasm despite a non-sarcastic instruction. DeepSeek generation failures leave 80 human-authored texts without generated counterparts. Some SarcTrain duplicates span validation folds and may affect checkpoint selection; these records do not enter DualTest. AuthTrain is generated with DeepSeek-R1-0528-Qwen3-8B, while Qwen3.5-9B replaces only DualTest's AI-generated subset. The result is directional test-set replication rather than retraining with another generator or broad generator-family coverage. The five source corpora also operationalize sarcasm differently, and the detector does not use conversational context.

The second set concerns comparison design and mechanism depth. DualTest supports both descriptive architecture comparison and subgroup diagnosis. The positive-class weight can affect absolute false-positive rates, although each matched joint–sarcasm-only comparison uses the same weight. The joint and sarcasm-only procedures differ in task schedule, total shared-encoder updates, and early stopping, so they estimate the complete joint-training procedure rather than a compute-matched auxiliary-gradient effect. Cross-architecture comparisons are not capacity- or optimization-matched, their MGTD-head learning rates differ, and the interaction-projection configuration lacks a matched sarcasm-only control. Representation diagnostics use one final plain joint model; source is also decodable from its matched sarcasm-only counterpart, and the interventions mainly redistribute non-sarcastic errors across sources rather than lowering their average.

The third set concerns the scale and proxy nature of generation evaluation. Predicted human-authorship probability and perceived human-likeness are related but non-equivalent constructs. Section 6 uses three LLM judges rather than human evaluators, and their instance-level agreement is low to moderate. The Chinese and English subsets differ in sample size, generation task, prompt structure, and source data. The analysis therefore shows condition-level directional differences between the evaluated detector scores and selected LLM-judge ratings, not human-perceived quality or a pure language effect. Priority extensions are independent annotation of AI-side style realization, capacity- and budget-matched controls, and multi-generator, multilingual, and calibration analyses.

## Ethics Statement <!-- optional; excluded from page limit -->

All human-authored texts used in this study come from publicly released research datasets (iSarcasm, CSC, ToSarcasm, News Headlines, and the Open Chinese Internet Sarcasm Corpus). Explicit repository licenses were available for iSarcasm and CSC; for sources without an identified redistribution license, we do not redistribute the human-authored text (Appendix A). We collect no new user data and did not conduct an additional personally identifiable information or offensive-content audit beyond the source-corpus preparation. AI-generated texts are produced locally using publicly released open-weight models, used only for training and evaluation, and explicitly labeled as AI-generated. The study recruits no new human participants but analyzes previously released human-authored text. API evaluation transmits the evaluation input, generated output, and rating instructions, without dataset metadata or ground-truth source labels. Given the observed subgroup error and the absence of validation for individual-text authorship attribution or high-stakes deployment, detector outputs should not be used as evidence of authorship or misconduct without human review and domain-specific validation. AI-assisted tools were used for grammar, wording, and formatting; they did not generate the reported experimental outputs or statistics, and the authors assume responsibility for the study and conclusions.

## References <!-- excluded from page limit; entries marked [TO VERIFY] must be checked before submission -->

1. Abu Farha, I., Oprea, S. V., Wilson, S., & Magdy, W. (2022). SemEval-2022 Task 6: iSarcasmEval, Intended Sarcasm Detection in English and Arabic. *Proceedings of SemEval-2022*.
2. Belrose, N., Schneider-Joseph, D., Ravfogel, S., Cotterell, R., Raff, E., & Biderman, S. (2023). LEACE: Perfect Linear Concept Erasure in Closed Form. *NeurIPS 2023*.
3. Benjamini, Y., & Hochberg, Y. (1995). Controlling the False Discovery Rate: A Practical and Powerful Approach to Multiple Testing. *JRSS: Series B*, 57(1), 289–300.
4. Bevendorff, J., et al. (2025). [TO VERIFY: MGTD should be understood as low-recall verification rather than a quality metric.] *Findings of ACL 2025*.
5. Casola, S., et al. (2024). MultiPICo: Multilingual Perspectivist Irony Corpus. *ACL 2024*.
6. Chai, H., & Wang, Z. (2025). Identifying and Mitigating Spurious Correlation in Multi-Task Learning. *CVPR 2025*. [TO VERIFY: full author names.]
7. Compton, R., et al. (2023). When More is Less: Incorporating Additional Datasets Can Hurt Performance by Introducing Spurious Correlations. [TO VERIFY: venue.]
8. Conneau, A., et al. (2020). Unsupervised Cross-lingual Representation Learning at Scale. *ACL 2020*.
9. DeepSeek-AI. (2025). DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning. *arXiv:2501.12948*.
10. Dettmers, T., Pagnoni, A., Holtzman, A., & Zettlemoyer, L. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. *NeurIPS 2023*.
11. Devlin, J., Chang, M.-W., Lee, K., & Toutanova, K. (2019). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. *NAACL-HLT 2019*.
12. Dugan, L., et al. (2024). RAID: A Shared Benchmark for Robust Evaluation of Machine-Generated Text Detectors. *ACL 2024*.
13. Elazar, Y., Ravfogel, S., Jacovi, A., & Goldberg, Y. (2021). Amnesic Probing: Behavioral Explanation with Amnesic Counterfactuals. *TACL*, 9, 160–175.
14. Geirhos, R., et al. (2020). Shortcut Learning in Deep Neural Networks. *Nature Machine Intelligence*, 2, 665–673.
15. Ghosh, A., & Veale, T. (2016). Fracking Sarcasm using Neural Network. *WASSA@NAACL 2016*.
16. Guo, B., et al. (2023). How Close is ChatGPT to Human Experts? *arXiv:2301.07597*.
17. Han, X., et al. (2022). [TO VERIFY: paper describing a leading iSarcasmEval system at SemEval-2022.]
18. He, H., & Choi, J. D. (2021). The Stem Cell Hypothesis: Dilemma behind Multi-Task Learning with Transformer Encoders. *EMNLP 2021*.
19. He, P., Gao, J., & Chen, W. (2023). DeBERTaV3: Improving DeBERTa using ELECTRA-Style Pre-Training with Gradient-Disentangled Embedding Sharing. *ICLR 2023*.
20. Hewitt, J., & Liang, P. (2019). Designing and Interpreting Probes with Control Tasks. *EMNLP-IJCNLP 2019*.
21. Hu, E. J., et al. (2022a). LoRA: Low-Rank Adaptation of Large Language Models. *ICLR 2022*.
22. Hu, Z., et al. (2022b). Improving Multi-Task Generalization via Regularizing Spurious Correlation. *NeurIPS 2022*.
23. Jang, H., & Frassinelli, D. (2024). Generalizable Sarcasm Detection is Just Around the Corner, of Course! *Proceedings of NAACL-HLT 2024*, 4238–4249. (CSC)
24. Jang, H., Braun, B., & Frassinelli, D. (2023). Intended and Perceived Sarcasm Between Close Friends: What Triggers Sarcasm and What Gets Conveyed? *Proceedings of the Annual Meeting of the Cognitive Science Society*, 45.
25. Karimi Mahabadi, R., Ruder, S., Dehghani, M., & Henderson, J. (2021). Parameter-efficient Multi-task Fine-tuning for Transformers via Shared Hypernetworks. *ACL 2021*.
26. Khatri, A., & P, P. (2020). Sarcasm Detection in Tweets with BERT and GloVe Embeddings. *FigLang@ACL 2020*.
27. Kumar, A., et al. (2017). [TO VERIFY: the hybrid CNN–LSTM sarcasm-detection model cited in §2.1.]
28. Li, D., et al. (2023). Interpreting and Exploiting Functional Specialization in Multi-Task Learning. [TO VERIFY: EMNLP 2023 or NeurIPS 2023.]
29. Liang, B., Lin, Z., Qin, B., & Xu, R. (2022). Topic-Oriented Sarcasm Detection: New Task, New Dataset and New Method. *Proceedings of the 21st Chinese National Conference on Computational Linguistics (CCL 2022)*, 557–568. (ToSarcasm)
30. Liu, B., Liu, X., Jin, X., Stone, P., & Liu, Q. (2021). Conflict-Averse Gradient Descent for Multi-task Learning. *NeurIPS 2021*.
31. Misra, R., & Arora, P. (2023). Sarcasm Detection using News Headlines Dataset. *AI Open*, 4, 13–18.
32. Misra, R., & Grover, J. (2021). *Sculpting Data for ML: The first act of Machine Learning*. ISBN 9798585463570.
33. Mitchell, E., et al. (2023). DetectGPT: Zero-Shot Machine-Generated Text Detection using Probability Curvature. *ICML 2023*.
34. Mueller, D., Dredze, M., & Andrews, N. (2022). Do Text-to-Text Multi-Task Learners Suffer from Task Conflict? *Findings of EMNLP 2022*.
35. Oprea, S., & Magdy, W. (2020). iSarcasm: A Dataset of Intended Sarcasm. *ACL 2020*.
36. Pfeiffer, J., et al. (2020). MAD-X: An Adapter-Based Framework for Multi-Task Cross-Lingual Transfer. *EMNLP 2020*.
37. Pfeiffer, J., et al. (2021). AdapterFusion: Non-Destructive Task Composition for Transfer Learning. *EACL 2021*.
38. Pilault, J., El hattami, A., & Pal, C. (2021). Conditionally Adaptive Multi-Task Learning. *ICLR 2021*.
39. Qwen Team. (2025). Qwen3 Technical Report. *arXiv:2505.09388*.
40. Ravfogel, S., Elazar, Y., Gonen, H., Twiton, M., & Goldberg, Y. (2020). Null It Out: Guarding Protected Attributes by Iterative Nullspace Projection. *ACL 2020*.
41. Riloff, E., et al. (2013). Sarcasm as Contrast between a Positive Sentiment and Negative Situation. *EMNLP 2013*.
42. Stickland, A. C., & Murray, I. (2019). BERT and PALs: Projected Attention Layers for Efficient Adaptation in Multi-Task Learning. *ICML 2019*.
43. Tay, Y., Tuan, L. A., Hui, S. C., & Su, J. (2018). Reasoning with Sarcasm by Reading In-between. *ACL 2018*.
44. Wang, C., Wu, R., & Yin, F. (2026). ViSP: A PPO-Enhanced Framework for Multimodal Sarcasm Generation with Contrastive Learning. *Neurocomputing*. [TO VERIFY: volume, issue, and pages; preprint arXiv:2507.09482.]
45. Wang, Y., et al. (2024). M4: Multi-generator, Multi-domain, and Multi-lingual Black-Box Machine-Generated Text Detection. *EACL 2024*.
46. Wen, C., Jia, G., & Yang, J. (2023). DIP: Dual Incongruity Perceiving Network for Sarcasm Detection. *Proceedings of CVPR 2023*, 2540–2550.
47. Wu, L., Rao, Y., Jin, H., Nazir, A., & Sun, L. (2019). Different Absorption from the Same Sharing: Sifted Multi-task Learning for Fake News Detection. *EMNLP-IJCNLP 2019*.
48. Wu, S., Zhang, H. R., & Ré, C. (2020). Understanding and Improving Information Transfer in Multi-Task Learning. *ICLR 2020*.
49. Yang, L., & Ikeda, D. (2023). The Impact of Language Properties in Multilingual Datasets on Sarcasm Detection. *Proceedings of the 14th IIAI International Congress on Advanced Applied Informatics (IIAI-AAI)*, IEEE, 1–6.
50. Yang, L., & Ikeda, D. (2024). The Influence of Linguistic Attribute Differences in Multilingual Datasets on Sarcasm Detection. *International Journal of Service and Knowledge Management*, 8(2).
51. Yao, ..., et al. (2024). SarcNet: A Multilingual Multimodal Sarcasm Detection Dataset. *LREC-COLING 2024*. [TO VERIFY: authors.]
52. Ye, W., et al. (2024). [TO VERIFY: survey on spurious correlations (“Clever Hans”).]
53. Yu, T., et al. (2020). Gradient Surgery for Multi-Task Learning. *NeurIPS 2020*.
54. Zhang, Z., et al. (2023). A Survey of Multi-task Learning in Natural Language Processing. *EACL 2023*.
55. Zheng, L., et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. *NeurIPS 2023*.
56. Zhu, Y. (2022). Open Chinese Internet Sarcasm Corpus Construction: An Approach. *Frontiers in Computing and Intelligent Systems*, 2(1), 7–9.

## Appendix A: Detailed Dataset Statistics and Quality Checks <!-- excluded from page limit; two columns -->

**A.1 Source details.**

| Source | Language | Instances | Genre / annotation method | Use |
|------|------|--------|------------------|------|
| iSarcasm (SemEval-2022 Task 6) | English | 3,467 | Tweets, speaker self-annotation | Sarcasm training + SFT |
| Cascade Sarcasm Corpus (CSC) | English | 6,878 | Conversational responses | Sarcasm training |
| ToSarcasm | Chinese | 3,898 | Topic comments, third-party annotation | Sarcasm training + SFT |
| News Headlines v2 | English | 28,619 | News headlines, distant supervision (The Onion / HuffPost) | Sarcasm training |
| Open Chinese Internet Sarcasm Corpus | Chinese | 2,000 | Weibo, third-party annotation | Sarcasm training |

The standardized merged source collection contains 44,862 instances in total (86.9% English / 13.1% Chinese). Chinese–English 1:1 balance is achieved through stratified sampling in the pipeline. The two English strata and the Chinese non-sarcastic stratum contain more than 2,500 available instances and are randomly downsampled within each stratum. The Chinese sarcastic stratum contains 2,396 source records representing 2,394 unique texts; 104 records are added by oversampling to reach 2,500. Including source-level duplicate text, the final stratum's text-duplication rate is 4.24%.

**Licensing and release scope.** The iSarcasm and CSC repositories state MIT licenses, and the News Headlines v2 Kaggle release states CC BY 4.0. We did not identify explicit redistribution licenses in the releases we accessed for ToSarcasm or the Open Chinese Internet Sarcasm Corpus. We therefore do not redistribute human-authored text from those two sources. Planned research artifacts are limited to eligible AI-generated text, labels and metadata, and reconstruction scripts that require users to obtain the human-authored corpora from their original sources.

**A.2 Three-dimensional distribution of DualTest.**

| Language | Source | Sarcastic | Non-sarcastic |
|------|--------|------|--------|
| English | Human | 200 | 1,200 |
| English | AI | 192 | 1,174 |
| Chinese | Human | 623 | 349 |
| Chinese | AI | 587 | 339 |

**A.3 Data quality checks.** Automated checks confirm language and label balance across the five SarcTrain folds. AuthTrain contains 9,996 complete human–AI pairs and three unmatched human instances. Record-level matching confirms that neither SarcTrain nor AuthTrain shares records with the human or AI side of DualTest; no train–test leakage is present. SarcTrain's text-duplication rate is 0.34% for English / 2.16% for Chinese. Chinese duplication is concentrated in the sarcastic stratum (4.24%; non-sarcastic stratum: 0.08%) and results from oversampling that stratum. Post-processing checks for the explicit chain-of-thought markers and reasoning prefaces described in A.4.

**A.4 AI-generated-text protocol.** The AI-generated texts in AuthTrain and the two DualTest versions are produced by locally deployed generation models through the Ollama API. DeepSeek-R1-0528-Qwen3-8B under tag `deepseek-r1:8b` generates AuthTrain and the DeepSeek version of DualTest's AI-generated subset; Qwen3.5-9B under tag `qwen3.5:9b` generates a second AI test subset that is evaluated with the same detectors without retraining. Both use temperature = 0.8, top-p = 0.95, and a generation limit of 500 tokens. Generation instructions use four templates defined by language × style, with style determined by the seed instance's sarcasm label. The templates constrain output style and length (20–80 Chinese characters; 10–50 English words). This is intended to reduce, but does not eliminate, source differences in length; Appendix E separately repeats the representation correlation analysis within length tertiles. The complete Chinese sarcastic template used in the experiment is reproduced verbatim below:

```
【任务】生成一条讽刺风格的评论
【主题】{seed}
【要求】
- 直接给出简短的评论，不要解释思路或分析
- 风格要讽刺、幽默、夸张
- 表达与主题相关的观点即可，不必追求严谨
- 20-80字为佳
【评论】
```

Its English translation is:

```
[Task] Generate a sarcastic-style comment
[Topic] {seed}
[Requirements]
- Directly provide a short comment; do not explain your reasoning or analysis
- The style should be sarcastic, humorous, and exaggerated
- It is sufficient to express a viewpoint related to the topic; strict rigor is not required
- Preferably 20–80 Chinese characters
[Comment]
```

The Chinese non-sarcastic version changes only the style line to “The style should be normal and natural.” The two English templates parallel the Chinese templates (the style lines are “sarcastic, humorous, exaggerated” and “normal, natural,” respectively; length constraint: 10–50 words). Post-processing comprises three steps: removing `<think>…</think>` chain-of-thought segments; removing reasoning-preface terms such as “好的 / 嗯 / Okay / Let me / First”; and removing Markdown markup. Failed generations are discarded. AuthTrain contains three generation failures (9,996 complete pairs; the corresponding human instances are retained). The DeepSeek-generated DualTest AI side likewise excludes a small number of failures; Qwen3.5 generation has zero failures, yielding 1,550 AI × non-sarcastic instances versus 1,513 for DeepSeek. For Qwen3.5 generation, thinking mode is explicitly disabled (Ollama `think = false`) to prevent the reasoning process from exhausting the output budget. Inline chain-of-thought from `deepseek-r1:8b` is removed through the post-processing described above.

**A.5 SFT training and generation evaluation.** Eligibility filtering retains 5,257 positive-sarcasm records suitable for generation tasks (2,972 English; 2,285 Chinese). A seed-42 shuffle and 40% sampling select 2,103 records (1,214 English; 889 Chinese); language balancing adds 325 oversampled Chinese records, producing GenTrain (2,428; 1,214 per language). By prompt type, it comprises 374 rewriting, 839 response, and 1,215 comment instances. Generation evaluation uses an independent set of 300 prompts (180 Chinese and 120 English): the Chinese task is to generate a sarcastic comment on a given topic, and the English task is to rewrite a normal sentence as a sarcastic one. The detector scoring and three blinded LLM evaluations in §6 are both conducted on this set.

## Appendix B: Hyperparameters and Training Details <!-- excluded from page limit; two columns -->

**B.1 Discriminative learning rates.**

| Experiment group | Encoder | Intermediate layer | Sarcasm head | MGTD head |
|--------|--------|--------|--------|----------|
| Plain dual-head / backbone comparison | $5\times10^{-7}$ | — | $5\times10^{-6}$ | $1\times10^{-5}$ |
| Intermediate variants | $5\times10^{-7}$ | $5\times10^{-6}$ | $5\times10^{-6}$ | $5\times10^{-6}$ |
| Single-head ablation | $5\times10^{-7}$ | (RCNN single-head: $5\times10^{-6}$) | $5\times10^{-6}$ | $1\times10^{-5}$ |

Note: the MGTD-head learning rate is not identical across all settings ($1\times10^{-5}$ for the plain dual-head and single-head models; $5\times10^{-6}$ for the architectural variants). Cross-configuration differences may therefore reflect optimization settings as well as intermediate-representation design. The matched dual–single comparisons retain the same settings within each architecture.

**B.2 Shared training hyperparameters.** AdamW with default PyTorch betas (0.9, 0.999) and epsilon 1e-8; parameter-group learning rates in B.1; weight decay 0.01; LambdaLR with 200 linear warmup steps and a constant rate thereafter; maximum 10 epochs; early-stopping patience 5; gradient clipping 1.0; physical batch size 4 × gradient accumulation 4 = effective batch size 16; sampling ratio MGTD:sarcasm = 2:1; sarcasm class weights [1.0, 2.5]; seed 42 plus fold index; fixed 90/10 MGTD-data holdout. Although SarcTrain is class-balanced, sarcasm detection is the primary task and the sarcastic class is its focal positive class, so errors on sarcastic examples receive greater weight. The normalized joint validation loss is 0.5 × [sarcasm validation loss / first-epoch value] + 0.5 × [MGTD validation loss / first-epoch value]. Final models use all applicable data, the mode of five best epochs + 1 (maximum 10), seed + 1000, and no early stopping. Single-head controls traverse the full sarcasm fold each epoch, whereas dual-head models draw sarcasm batches under the 2:1 schedule.

**Model sizes, software, and hardware environment.** The encoders have approximately 178M (mBERT), 278M (XLM-RoBERTa-base), 184M (DeBERTa-v3-base), and 435M (DeBERTa-v3-large) parameters; the SFT generator Qwen3-8B has about 8B parameters, of which QLoRA updates only low-rank adapters. Detector training and inference are performed on a single NVIDIA GeForce RTX 3090 (24 GB); the five-fold training of the backbone comparison, architecture, and ablation variants together with the final model amounts to on the order of tens of GPU-hours, with a single detector run taking from a few minutes to a few hours and SFT about two hours. The software environment is PyTorch 2.9.0 (CUDA 12.8), transformers 4.57.1, and peft 0.18.0. AI-generated texts are produced locally through Ollama (Appendix A.4); LLM evaluation without ground-truth source labels uses API calls made in May 2026 (Appendix C.4).

**B.3 SFT (QLoRA).** Qwen3-8B; 4-bit loading (bfloat16 computation); LoRA rank 64 / alpha 16 / dropout 0; target modules q/k/v/o_proj and gate/up/down_proj; maximum sequence length 2,048; gradient checkpointing enabled; learning rate $5\times10^{-5}$; 3 epochs; batch size 1 × gradient accumulation 8; warmup ratio 0.05. After language balancing, a seed-42 split holds out 15% for validation; validation loss selects the checkpoint.

## Appendix C: Complete Experimental Results <!-- excluded from page limit; two columns -->

**C.1 Macro-averaged F1 (supplement to Table 1).** RCNN macro-averaged F1, under the same five-run protocol and calculated from each run's confusion matrix, is **0.6678 ± 0.036** for sarcasm and **0.9298 ± 0.005** for MGTD. Sarcasm macro-F1 is higher than positive-class F1 (0.6309) because the majority class has a higher F1. These values supplement rather than rerank the full positive-class comparison in §5.1.

**C.2 Complete four-quadrant results for the MGTD head (including single-head controls).** The reporting convention matches Table 3 (recall for human subsets; false-positive rate for AI subsets).

| Model | Human × sarcastic recall | Human × non-sarcastic recall | AI × sarcastic false positive | AI × non-sarcastic false positive |
|------|----------------|------------------|--------------|----------------|
| Plain dual-head | 91.4% | 91.5% | 3.6% | 6.1% |
| Interaction-projection dual-head | 85.4% | 85.4% | 1.8% | 4.1% |
| RCNN dual-head | 94.8% | 93.2% | 5.5% | 6.6% |
| Plain single-head MGTD | 90.4% | 86.6% | 4.1% | 4.2% |
| RCNN single-head MGTD | 90.5% | 92.4% | 3.6% | 5.7% |

The single-head MGTD controls remain within single-digit percentage points of the dual-head models in every quadrant. We do not observe an MGTD subgroup error increase comparable to that of the sarcasm head under the reported metrics. On the Qwen3.5-9B replication set, AI × non-sarcastic false-positive rates are 0.9% / 1.3% / 0.5% for the plain / interaction-projection / RCNN configurations, respectively; AI × sarcastic rates are all $\leq 0.4\%$; and overall MGTD F1 scores are 0.952 / 0.917 / 0.966.

**C.3 Complete source × sarcasm results: confidence intervals, paired tests, language strata, and generator transfer (§5.2–§5.3).** For each separately trained final model, this section quantifies uncertainty from the test instances with Wilson 95% intervals for quadrant rates and 95% percentile intervals from 10,000 instance-level bootstrap samples for F1. These intervals complement the between-run standard deviations in Tables 1 and 2.

Wilson 95% intervals for the sarcasm false-positive rate on the AI × non-sarcastic subset (two generators; unit: %):

| Model | deepseek-r1:8b | qwen3.5:9b |
|------|----------------------|--------------------|
| Plain dual-head | 86.3 [84.5, 88.0] | 91.3 [89.8, 92.6] |
| Interaction-projection dual-head | 40.9 [38.5, 43.4] | 29.5 [27.3, 31.8] |
| RCNN dual-head | 34.1 [31.8, 36.5] | 22.1 [20.1, 24.3] |
| Plain single-head | 19.3 [17.4, 21.4] | 7.9 [6.6, 9.3] |
| RCNN single-head | 19.3 [17.4, 21.4] | 10.6 [9.2, 12.3] |

Paired McNemar tests use predictions on the same DeepSeek-generated AI × non-sarcastic texts. Discordant pairs are 1,021 / 7 for plain dual-head vs. plain single-head, 239 / 15 for RCNN dual-head vs. RCNN single-head, 805 / 15 for plain dual-head vs. RCNN dual-head, and 203 / 100 for interaction-projection dual-head vs. RCNN dual-head (all $p < .001$).

Per-language and human-subset sarcasm F1 (final models; bootstrap 95% intervals included for RCNN):

| Model | Full set | Chinese | English | Human subset | Human×zh | Human×en |
|------|------|------|------|----------|---------|---------|
| Plain dual-head | 0.556 | 0.746 | 0.312 | 0.586 | 0.710 | 0.374 |
| Interaction-projection dual-head | 0.643 | 0.781 | 0.420 | 0.593 | 0.739 | 0.366 |
| RCNN dual-head | 0.663 [0.646, 0.680] | 0.819 [0.802, 0.836] | 0.429 [0.399, 0.458] | 0.629 [0.605, 0.653] | 0.774 [0.748, 0.798] | 0.399 [0.355, 0.440] |
| Plain single-head | 0.661 | 0.750 | 0.514 | 0.612 | 0.719 | 0.434 |
| RCNN single-head | 0.664 | 0.758 | 0.511 | 0.603 | 0.709 | 0.434 |

RCNN MGTD F1 is 0.938 [0.931, 0.945] on the full set, 0.961 [0.952, 0.969] in Chinese, and 0.923 [0.912, 0.933] in English. On the human subset, joint-versus-sarcasm-only differences are within 0.03 and vary in direction; the negative-transfer loss is concentrated in quadrants containing AI-generated texts. For the plain dual-head model on AI × non-sarcastic texts, the median sarcasm probability is 0.767 (interquartile range 0.635–0.839); the single-head medians are 0.204 (plain) and 0.069 (RCNN). Similar within-AI AUC values of 0.82–0.88 show that internal ranking is largely preserved.

**C.4 Blinded-evaluation protocol, inter-judge agreement, and group-level tests (§6).** Protocol: each sample is sent to each of the three judge APIs independently with a single prompt (May 2026; temperature = 0.3; GPT-5-nano does not support a temperature parameter and uses its default decoding). The prompt contains only the input and the text to be evaluated, without ground-truth source information, and asks the judge to infer the source and then provide two scores from 1 to 5 for sarcasm and human-likeness. Base and SFT outputs are evaluated independently with the same prompt. The judges were accessed through the model identifiers `deepseek-chat`, `gemini-2.5-flash-lite`, and `gpt-5-nano`; DeepSeek's April 24, 2026 API announcement identifies `deepseek-chat` as routing to the non-thinking mode of DeepSeek-V4-Flash. Pairwise instance-level agreement between judges is moderate to low: Spearman $\rho$ is 0.20–0.38 for sarcasm (all $p < 10^{-6}$) and 0.03–0.23 for human-likeness (one pair not significant), so individual-text rankings are not treated as a common gold standard. The condition-level observation rests separately on the consistent rating direction across all six language × judge groups. The pooled mean is a descriptive aggregate; judge-specific results remain necessary because agreement is limited.

Group-level tests for blinded sarcasm judgments (Wilcoxon + BH-FDR):

| Evaluator / language | $n$ | SFT mean | Base mean | Adjusted $p$ | Significant |
|---------------|---|----------|----------|----------|------|
| DeepSeek-V4-F / English | 120 | 3.292 | 4.025 | < 0.0001 | Yes |
| DeepSeek-V4-F / Chinese | 180 | 3.878 | 4.044 | 0.0008 | Yes |
| Gemini-2.5-FL / English | 120 | 3.750 | 3.992 | 0.0008 | Yes |
| Gemini-2.5-FL / Chinese | 180 | 3.994 | 4.028 | 0.2008 | **No** |
| GPT-5-nano / English | 119 | 3.681 | 4.084 | < 0.0001 | Yes |
| GPT-5-nano / Chinese | 180 | 3.517 | 3.911 | < 0.0001 | Yes |

Note: GPT-5-nano lacks one English base rating, giving $n=119$ in C.4, and three Chinese human-reference ratings, giving $n=177$ in C.5 without affecting the C.4 Chinese row. Tests use pairwise deletion.

**C.5 Paired tests between SFT and human references (supplementary).** This test is performed on the Chinese subset of the 300-prompt generation evaluation ($n = 180$; GPT-5-nano is missing three instances, leaving 177). These comparisons provide judge- and dimension-specific calibration context: for DeepSeek-V4-Flash, SFT is significantly below the reference on human-likeness (−0.161, $p = 0.0025$), whereas for GPT-5-nano, SFT is significantly above the reference on sarcasm (+0.186, $p = 0.0114$). The remaining four comparisons do not reach significance at the reported threshold. The observed reference differences therefore vary across judges and dimensions; these tests do not estimate a judge-by-dimension interaction.

**C.6 Plain-detector control.** Scores: Chinese sarcasm 0.765 → 0.763 (−0.001; no change), English sarcasm 0.843 → 0.659 (−0.184); MGTD increases by +0.760 in Chinese and +0.705 in English. Correlations are overall sarcasm $\rho = +0.142$ / MGTD $−0.214$; Chinese sarcasm $−0.037$ (not significant) / MGTD $−0.209$; English sarcasm $+0.342$ / MGTD $−0.283$. The two detectors agree on the rise in predicted human-authorship probability. The Chinese sarcasm-probability change is detector-sensitive, whereas both detectors show a decrease in English.

**C.7 Within-condition correlations (RCNN detector, recomputed separately within the base and SFT conditions).** MGTD dimension: within the base condition, $\rho = +0.03$ ($n = 300$, not significant; Chinese $+0.10$ / English $−0.09$, neither significant); within the SFT condition, $−0.12$ ($p = 0.039$; Chinese $−0.19$, $p = 0.009$ / English $−0.12$, not significant). The pooled-sample correlation of $−0.212$ is driven mainly by the opposite between-condition changes (SFT increases the detector score by approximately 0.67 while decreasing the mean blinded judgment); there is no positive alignment within conditions. Sarcasm dimension: within-condition correlations for Chinese are base $+0.05$ (not significant) / SFT $+0.15$ ($p = 0.048$), and for English are base $+0.41$ / SFT $+0.37$ (both significant). The Chinese within-condition correlation is at most weakly positive, consistent with the near-zero pooled result.

**C.8 Detector scores before and after SFT across the two task–language settings.** Scores from the RCNN detector for base and SFT outputs (0–1):

| Language | Condition | Sarcasm score | MGTD score |
|------|------|----------|------------|
| Chinese | Base → SFT | 0.780 → 0.895 (+0.115) | 0.289 → 0.969 (+0.680) |
| English | Base → SFT | 0.854 → 0.730 (−0.124) | 0.198 → 0.864 (+0.666) |

Sarcasm scores move in opposite directions in the Chinese comment-generation and English rewriting settings (+0.115 / −0.124). GenTrain itself is balanced at 1,214 instances per language; the evaluation settings differ in task, prompt format, source data, and sample size. Mean judgments from the three LLMs (sarcasm / human-likeness) are 3.77/4.17 for the human reference, 4.01/4.24 for the base model, and 3.71/4.08 for SFT. MGTD scores rise sharply in both settings, so the detector assigns SFT outputs higher predicted human-authorship probabilities while the LLM-judge ratings of perceived human-likeness move in the opposite direction.

## Appendix D: Illustrative Cases of Disagreement between Detector Probabilities and LLM-Judge Ratings <!-- excluded from page limit; two columns -->

Illustrative cases were manually selected after inspecting the paired table of RCNN-detector probabilities and ratings from the three LLM judges to show large disagreements; no formal ranking or top-k selection was implemented. Detector probabilities range from 0 to 1, and LLM-judge ratings are the three-evaluator mean on a 1–5 scale; long texts are truncated. Chinese examples retain the original model input and provide an English translation in brackets.

**Assigned high predicted human-authorship probability by the detector but lower LLM-judge ratings of perceived human-likeness (corresponding to §6).**

| Language | Source | $P(\text{human})$ | LLM-judge human-likeness rating | Text (excerpt) |
|------|------|---------|----------|--------------|
| Chinese | SFT | 1.00 | 2.67 | “好，我需要针对三星回怼华为的新闻生成一条讽刺评论。首先……” [“Okay, I need to generate a sarcastic comment about the news that Samsung hit back at Huawei. First…”] (chain-of-thought leakage) |
| Chinese | SFT | 1.00 | 2.33 | “跑分竞赛：参数焦虑：跑分焦虑：水货：哈哈哈” [“Benchmark competition: spec anxiety: benchmark anxiety: shoddy goods: hahaha”] (degenerate output) |
| English | SFT | 1.00 | 2.67 | “Same as me, I'm not sure” (vacuous short sentence) |

In the first case, AI-generated text with obvious chain-of-thought traces is nevertheless assigned predicted human-authorship probability 1.00 by the MGTD head, corresponding to the disagreement discussed in §6.

**Detector-high / LLM-judge-low sarcasm cases.**

| Language | Source | $P(\text{sarcastic})$ | LLM-judge sarcasm rating | Text (excerpt) |
|------|------|---------|----------|--------------|
| Chinese | SFT | 1.00 | 2.67 | “三星：我才是第一！你看看我这摄像头总分，109+96，112+89，我赢了！” [“Samsung: I'm the real number one! Look at my camera totals: 109+96, 112+89—I won!”] (numbers repeated without sarcastic transformation) |
| Chinese | SFT | 0.99 | 2.67 | “恭喜恭喜，美国牛牛牛，继续加油” [“Congratulations, congratulations, America is amazing, keep it up.”] (formulaic mock-praise construction) |
| English | SFT | 0.93 | 2.67 | “2 hours left to tip it over, I'm going to the toilet” |

**Detector-low / LLM-judge-high sarcasm cases.**

| Language | Source | $P(\text{sarcastic})$ | LLM-judge sarcasm rating | Text (excerpt) |
|------|------|---------|----------|--------------|
| Chinese | Base | 0.14 | 4.00 | “特朗普承诺 120 亿补贴像撒大饼，农民收到的却是缩水成硬币的承诺——原来政治承诺比玉米收成更难变现。” [“Trump's promise of 12 billion in subsidies was like tossing out a giant pie, but what farmers received was a promise shrunk to the size of a coin—apparently political promises are harder to cash in than a corn harvest.”] (metaphorical irony) |
| Chinese | Base | 0.24 | 4.00 | “特斯拉：价格跳水后又跳水，消费者：钱包跳水后又跳水。” [“Tesla: prices dive and dive again; consumers: wallets dive and dive again.”] (parallel-structure irony) |
| English | Base | 0.12 | 4.00 | “Thank you so much for this \*beloved\* review, Mandy! I'm \*overjoyed\* you enjoyed it” (asterisk emphasis) |

Taken together, these manually selected cases motivate the hypothesis that surface markers such as mock-praise formulas, exclamation marks, and colloquial short sentences contribute to some detector decisions, while semantic reversal contributes to some LLM-judge ratings. They define candidate features for systematic prevalence and feature-level testing.

## Appendix E: Representation-Level Analysis of Error Distribution between Source Groups <!-- excluded from page limit; two columns -->

This appendix records the complete protocol, data, and self-checks for the representation-level tests in §5.4.

**E.1 Setup.** The primary diagnostics use the frozen model that produces the 86.3% result for the plain dual-head model in the main text; the matched sarcasm-only model is used only for the source-probe comparison. The full-set probe, correlation, and alignment analyses use all applicable DualTest examples. For interventions, the DualTest examples are divided into two disjoint halves; this is a split over examples, not representation coordinates, and each example retains its complete vector. The fitting half estimates directions or erasure transformations, which are then applied without refitting to the evaluation half. False-positive rates use the original sarcasm head, whereas residual source AUC uses five-fold cross-validation of a newly fitted source probe within the transformed evaluation representations. The intervention baseline (84.8%) therefore differs from the full-set value (86.3%). Code self-checks show that manually computed logits match model outputs elementwise (maximum error $1.4 \times 10^{-6}$), the residual after erasure projection is approximately $10^{-6}$, the difference between class means on the fitting half has relative residual $7 \times 10^{-6}$ after LEACE, and a linear probe evaluated on that half is at chance (AUC 0.514). The probe AUC is 0.986 with true labels and 0.512 with permuted labels.

**E.2 Protocol.** Section 5.4 gives the principles and citations for each test; implementation details are recorded here. The linear probe is logistic regression with five-fold cross-validation. The selectivity control retrains the same probe with permuted source labels. Correlation analyses are rechecked after stratification by text-length tertile. For alignment, $\Sigma$ is the Ledoit–Wolf covariance matrix fitted to all pooled DualTest representations, with eigenvalues clipped below $10^{-8}$; each $w$ is a task-head weight difference or probe direction. For two such directions $w_1$ and $w_2$, whitened alignment is $\cos(\Sigma^{1/2}w_1,\Sigma^{1/2}w_2)$. Erasure interventions include a single direction, INLP subspaces with $k = 1$–$16$ removed directions, and LEACE. Controls comprise ten sampled random whitened directions and erasure after re-estimating directions from permuted labels. The exploratory, study-specific INLP-3 residual PC1 asks whether a high-variance direction outside the fitted three-direction INLP source subspace can affect subgroup error allocation. It is the maximum-variance direction after projecting out those fitted directions; unchanged linear source decodability does not imply independence from all multidimensional or nonlinear source information.

**E.3 Diagnostic evidence.** Source is highly linearly decodable: AUC 0.986 (sarcastic subset: 0.990 / non-sarcastic subset: 0.985; permuted-label control: 0.512; single-head model: 0.974, establishing decodability as a background property shared by both models). On AI × non-sarcastic instances, the MGTD-head AI logit and sarcasm-head logit correlate at $\rho = 0.519$ (0.44/0.56/0.63 by text-length tertile). The sarcasm decision direction and source direction are nearly orthogonal in the original geometry ($\cos \approx -0.03$) but moderately aligned in whitened geometry ($|\cos| = 0.41$ when measured using the probe direction / 0.49 using the MGTD-head direction). These results provide correlational evidence linking source-related structure to the affected sarcasm decision and motivate the intervention tests below.

**E.4 Intervention evidence.** $\text{gap} =$ AI × non-sarcastic false-positive rate − human × non-sarcastic false-positive rate; source AUC is the decoding ability of a probe retrained after erasure.

| Erasure scheme | AI × non-sarcastic false positive | Human × non-sarcastic false positive | Gap | Source AUC (after) |
|----------|----------------|------------------|------|----------------|
| None (baseline) | 84.8 | 32.8 | 52.0 | 0.982 |
| Single direction (MGTD head / independent probe / random) | 84.8–86.2 | — | — | ≈ 0.98 |
| INLP subspace ($k = 16$) | 89.8 | — | — | 0.974 |
| **LEACE (linear erasure)** | 68.7 | 49.5 | 19.1 | **0.589** |
| Sampled random whitened ×10 (control) | 84.9 | 32.7 | — | 0.982 |
| **INLP-3 residual PC1** | **66.7** | 54.1 | 12.6 | **0.982** |

Bold marks the two diagnostic interventions (LEACE and INLP-3 residual PC1), not best performance. Interpretation: single-direction and low-dimensional INLP erasure produce little change. LEACE reduces measured linear source decodability and the false-positive-rate difference between source groups from 52 to 19, while the ten sampled random-direction controls show no comparable change. Erasing INLP-3 residual PC1 changes the AI-side rate to 66.7 and the difference between source groups to 12.6 while leaving measured linear source decodability unchanged. Residual source AUC comes from a newly fitted probe, whereas the FPR gap comes from the frozen sarcasm head; their divergence is consistent with source remaining recoverable from other structure while the removed direction shifts the groups differently relative to the fixed sarcasm boundary. The result identifies sensitivity to this residual direction and makes its semantic content a target for further analysis. Under LEACE, the unweighted mean false-positive rate across the two non-sarcastic source groups is almost unchanged (58.8 → 59.1), showing that the affected directions shape how errors are allocated between AI and human texts.
