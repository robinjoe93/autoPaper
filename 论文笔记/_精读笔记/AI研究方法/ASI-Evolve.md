---
title: "ASI-Evolve: AI Accelerates AI"
method_name: "ASI-Evolve"
authors: [Weixian Xu, Tiantian Mi, Yixiu Liu, Yang Nan, Zhimeng Zhou, Lyumanshan Ye, Lin Zhang, Yu Qiao, Pengfei Liu]
year: 2026
venue: arXiv
tags: [ai-for-ai, evolutionary-search, agent-system, neural-architecture-search, data-curation, rl-algorithm]
zotero_collection: AI研究方法
image_source: local
arxiv_html: https://arxiv.org/html/2603.29640v1
created: 2026-04-03
---

# 论文笔记：ASI-Evolve: AI Accelerates AI

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | SJTU (上海交通大学) / SII / GAIR |
| 日期 | 2026年3月 |
| 项目主页 | https://github.com/GAIR-NLP/ASI-Evolve |
| 对比基线 | [[DeltaNet]], [[GRPO]], [[DrugBAN]] |
| 链接 | [arXiv](https://arxiv.org/abs/2603.29640v1) / [Code](https://github.com/GAIR-NLP/ASI-Evolve) |

---

## 一句话总结

> ASI-Evolve 是首个在**数据、架构、算法**三个核心AI组件上展示统一突破的AI-for-AI框架，通过learn–design–experiment–analyze闭环实现自主科学研究，发现105个SOTA架构并将MMLU提升超18分。

---

## 核心贡献

1. **首创AI-for-AI统一框架**: 首次在模型架构、预训练数据、训练算法三大核心组件上实现AI驱动的统一突破
2. **Learn–Design–Experiment–Analyze闭环**: 引入认知库(Cognition Base)注入人类先验、Analyzer提炼复杂实验结果，实现长周期研究的有效迭代
3. **显著实证成果**: 发现105个超越DeltaNet的架构、数据整理提升MMLU+18.64分、RL算法提升AMC32+12.5分，并验证跨领域迁移能力

---

## 问题背景

### 要解决的问题

**核心问题**: AI能否加速AI自身的研发？

AI进展依赖于数据、模型架构、学习算法三大组件的反复迭代（假设生成→实现→实验→分析），但这些循环受限于人类瓶颈：
- 假设空间的并行探索能力严重受限
- 实验流程需要大量人工干预
- 跨迭代的知识积累依赖个人经验，难以系统化保存和迁移

### 现有方法的局限

| 方法类别 | 代表系统 | 局限性 |
|----------|----------|--------|
| 科学问答 | SciMaster, GPQA, HLE | 无实验执行，仅回答已知问题 |
| 结构化任务执行 | MLE-bench, SWE-bench, AI Scientist | 目标预定义，非开放发现 |
| 轻量科学发现 | AlphaEvolve, FunSearch, OpenEvolve | 单函数级修改，反馈简单直接 |

现有框架无法处理**高成本、长周期、弱监督**的真实AI研究循环：修改大代码库、运行昂贵实验、解释多维反馈、跨轮次保持探索一致性。

### 本文的动机

真实AI研究任务的复杂度远超现有系统能处理范围，需要：
- 从一开始注入领域先验知识，避免盲目搜索
- 专门解释复杂多维实验反馈
- 持续积累和复用洞察

因此设计 **Cognition Base**（注入人类先验）和 **Analyzer**（提炼复杂结果），实现"演化认知本身而非仅演化解"。

---

## 方法详解

### 任务复杂度框架

论文提出 **Ltask = ⟨Cexec, Sspace, Dfeedback⟩** 框架系统刻画科研任务复杂度：

$$
L_{task} = \langle C_{exec}, S_{space}, D_{feedback} \rangle
$$

**含义**: 科学任务的内在挑战由三个维度构成

**符号说明**:
- $C_{exec}$: 执行成本——每次尝试的计算资源与工程复杂度（修改大代码库、GPU小时）
- $S_{space}$: 搜索空间复杂度——目标开放性、边界预定义程度、探索方向需发现而非给定
- $D_{feedback}$: 反馈复杂度——从实验结果提取可行动洞察的难度（需综合多维信号）

### 整体架构

ASI-Evolve 采用 **Agent系统** 架构，包含四个核心模块：

- **输入**: 任务描述 + 认知库查询
- **核心模块**: [[Researcher]]（设计）→ [[Engineer]]（执行）→ [[Analyzer]]（分析）
- **存储**: [[Cognition Base]]（人类先验）+ [[Database]]（演化历史）
- **输出**: 改进的候选程序 + 结构化分析报告

### 核心模块

#### Researcher (研究者)

**设计动机**: 基于[[Evolutionary Search|进化搜索]]范式生成候选程序

**具体实现**:
- 从数据库采样 $n$ 个节点 $S_t \sim \text{Sample}(D)$
- 通过语义检索获取相关认知条目 $R_t = \text{Retrieve}(C; S_t)$
- LLM生成新候选程序及其动机描述：

$$
p_t \sim P(p | S_t, R_t)
$$

**符号说明**:
- $p_t$: 第 $t$ 轮生成的程序
- $S_t$: 采样的历史节点
- $R_t$: 检索的认知条目
- $P$: 程序生成概率分布

支持两种生成模式：
- **全代码生成**: 完整生成新程序
- **Diff编辑**: 增量修改父程序（适合大代码库长期演化）

#### Engineer (工程师)

**设计动机**: 在实际实验环境中执行程序并产生评估信号

**具体实现**:
- 调用用户指定的评估脚本，端到端运行实验
- 返回结构化指标（包括主评分作为fitness信号）
- 支持**早拒绝**机制：wall-clock限制 + 快速测试，过滤瑕疵候选
- 可选[[LLM-as-a-Judge]]评估难以规则捕获的质量维度

#### Analyzer (分析器)

**设计动机**: 处理"简单评分 vs 复杂反馈"的不对称性

**具体实现**:
- 接收程序 + 完整实验输出（日志、多指标、训练动态）
- 提炼为紧凑、决策导向的报告
- 报告持久化到数据库，供后续轮次检索
- 保持上下文长度可控，不牺牲分析深度

#### Cognition Base (认知库)

**设计动机**: 为长周期研究注入[[Human Prior|人类先验知识]]

**具体实现**:
- 编码任务相关启发式、已知陷阱、设计原则
- 每轮采样后，用节点信息查询语义相似条目
- 注入Researcher上下文引导假设生成
- **关键作用**: 加速冷启动爬升，不限制长期探索

#### Database (数据库)

**设计动机**: 系统持久化记忆，存储演化历史

**每个节点存储**:
- (i) Researcher动机
- (ii) 生成的程序
- (iii) 结构化评估结果
- (iv) 分析报告
- (v) 元数据（运行时间、成功标志）

**采样策略**（统一接口）:
- [[UCB1 Sampling]]: 上置信界选择，平衡价值与探索
- [[MAP-Elites]]: 质量-多样性档案，保持多维度解空间覆盖
- Random: 均匀随机
- Greedy: 贪婪选最高分

---

## 关键公式

### 公式1: [[Scientific Task Length|任务复杂度刻画]]

$$
L_{task} = \langle C_{exec}, S_{space}, D_{feedback} \rangle
$$

**含义**: 三维框架刻画科研任务的内在挑战，区分不同自动化系统的能力边界

**符号说明**:
- $C_{exec}$: 执行成本（GPU小时、代码库修改复杂度）
- $S_{space}$: 搜索空间复杂度（目标开放性、边界预定义度）
- $D_{feedback}$: 反馈复杂度（多维信号综合难度）

### 公式2: [[Program Generation|程序生成条件]]

$$
p_t \sim P(p | S_t, R_t)
$$

**含义**: 新候选程序基于历史节点和认知条目条件生成

**符号说明**:
- $p_t$: 第 $t$ 轮生成的程序
- $S_t \sim \text{Sample}(D)$: 从数据库采样的节点
- $R_t = \text{Retrieve}(C; S_t)$: 从认知库检索的相关条目

### 公式3: [[UCB1 Sampling|UCB1采样选择]]

$$
\text{UCB1}(n) = \hat{Q}(n) + c \sqrt{\frac{\ln N}{n_{visits}}}
$$

**含义**: 平衡已观察价值与探索潜力，选择下一个父节点

**符号说明**:
- $\hat{Q}(n)$: 节点 $n$ 的估计价值（平均得分）
- $c$: 探索常数（论文用 $c = 1.414$）
- $N$: 总采样次数
- $n_{visits}$: 节点 $n$ 的访问次数

### 公式4: [[Pairwise Asymmetric Optimization|成对不对称优化]] (Algorithm A)

**优势估计**:

$$
A_i = \frac{1}{n-1} \sum_{j \neq i} \tanh(R_i - R_j)
$$

**含义**: 用成对奖励差值的tanh归一化替代组均值，更鲁棒

**符号说明**:
- $A_i$: 样本 $i$ 的优势值
- $R_i, R_j$: 样本 $i, j$ 的奖励
- $n$: 组内样本数

**不对称裁剪**:

$$
\epsilon_{down} = \begin{cases} \epsilon_0 & \text{if } A > 0 \\ k\epsilon_0 & \text{if } A < 0 \end{cases}
$$

**含义**: 根据优势符号动态调整PPO裁剪窗口

### 公式5: [[Budget-Constrained Dynamic Radius|预算约束动态半径]] (Algorithm B)

**优势归一化**:

$$
\hat{A} = \frac{r - c}{s}
$$

**含义**: 基于百分位的归一化，其中 $c$ 为中位数、$s$ 为IQR

**全局更新预算约束**:

$$
\exp(c) \times |A| \leq z_{cap}
$$

**含义**: 数学保证总策略更新幅度在预算内，稳定训练

---

## 关键图表

### Figure 1: Scientific Task Length Space / 任务复杂度空间

![[ASI-Evolve_fig1_ltask_space.png]]

**说明**: 展示不同科研自动化系统在 $L_{task} = \langle C_{exec}, S_{space}, D_{feedback} \rangle$ 三维空间中的位置。ASI-Evolve 目标任务（架构设计、数据整理、RL算法）位于高复杂度区域，现有系统未曾触及。

### Figure 2: ASI-Evolve Pipeline / 系统流程

![[ASI-Evolve_fig2_pipeline.png]]

**说明**: ASI-Evolve 的完整流程：每轮从数据库采样节点→检索认知条目→生成候选程序→执行评估→分析报告存回数据库。突出 Cognition Base 注入和 Analyzer 消化复杂反馈的关键作用。

### Figure 3: Framework & Model Comparison / 框架与模型对比

**Figure 3(a): 框架对比**

![[ASI-Evolve_fig3a_framework_comparison.png]]

**说明**: ASI-Evolve vs GEPA vs OpenEvolve 在 circle packing 任务上的演化曲线。ASI-Evolve 冷启动更快、持续改进、唯一可靠达到SOTA（2.636）。

**Figure 3(b): 模型对比**

![[ASI-Evolve_fig3b_model_comparison.png]]

**说明**: GPT-5-mini vs Qwen3-32B 作为基座模型的演化曲线。两者收敛到相似范围，表明框架能力不依赖特定模型家族。

### Figure 4: Sampling Algorithm Comparison / 采样算法对比

![[ASI-Evolve_fig4_algorithm_comparison.png]]

**说明**: MAP-Elites vs UCB1 vs Random 采样策略对比。Random 初期高但后期落后；UCB1 在认知辅助下快速收敛；配合 GPT-5-mini 仅17轮达到SOTA。

### Figure 5: Ablation Curves / 消融实验

![[ASI-Evolve_fig5_ablation_curves.png]]

**说明**: Full Method vs No Analyzer vs No Cognition 消融曲线。Cognition 提供冷启动优势；Analyzer 保证持续改进能力；两者缺一则演化受限。

### Table 1: Architecture Design Benchmarks / 架构设计基准

| Benchmarks | DeltaNet | Gated-DeltaNet | Mamba2 | PG | C | FG | H | AM |
|------------|----------|----------------|--------|----|----|----|----|----|
| **Development** |
| Wiki ppl↓ | 17.00 | 16.84 | 16.66 | 16.18 | 16.05 | 15.77 | 16.65 | 16.26 |
| LMB ppl↓ | 13.63 | 13.31 | 13.33 | 12.62 | 13.45 | **12.34** | 13.06 | 13.75 |
| LMB acc | 45.47 | 46.26 | 46.24 | 47.60 | 46.13 | **47.53** | 46.56 | 45.04 |
| PIQA | 73.12 | 74.10 | 73.78 | 72.91 | 74.37 | 72.91 | 74.37 | 74.10 |
| Hella | 56.29 | 57.55 | 58.58 | 56.99 | 57.00 | **58.47** | 56.85 | 57.17 |
| Wino | 55.88 | 58.01 | 58.48 | 57.22 | 57.85 | **60.14** | 57.38 | 57.62 |
| ARC-e | 73.40 | 72.14 | 72.98 | 73.06 | 72.05 | **74.28** | 73.11 | 74.28 |
| ARC-c | 40.61 | 36.95 | 39.33 | **40.36** | 39.76 | 40.02 | 39.33 | 39.33 |
| SIQA | 40.74 | 41.71 | 41.81 | 42.37 | 41.81 | **42.78** | 42.07 | 42.07 |
| BoolQ | 60.58 | 53.98 | 60.52 | 62.45 | **62.51** | 62.11 | **63.03** | 56.27 |
| **Generalization** |
| RACE | 34.45 | 33.78 | 32.15 | **35.22** | 34.55 | 35.60 | 35.22 | 35.02 |
| BBQ | 29.53 | 29.75 | 29.43 | 29.95 | 30.55 | **31.46** | 30.88 | 30.27 |
| MetaBench | 26.97 | 28.67 | 27.70 | 25.64 | **29.55** | 26.79 | 29.38 | 28.98 |
| QA4MRE | 40.00 | 35.00 | 39.17 | **39.17** | **39.17** | 38.33 | 38.33 | 38.33 |
| SCIQ | 89.80 | 90.30 | 90.30 | 89.60 | 89.50 | 89.20 | **90.40** | 89.20 |
| SWAG | 47.69 | 48.17 | 48.88 | 48.22 | 47.80 | **48.57** | 48.18 | 47.80 |
| **Averages** |
| Dev. Avg | 55.76 | 55.09 | 56.47 | 56.62 | 56.44 | **57.28** | 56.59 | 55.74 |
| Gen. Avg | 44.74 | 44.28 | 44.61 | 44.63 | **45.19** | 44.99 | **45.40** | 44.93 |
| Overall Avg | 51.04 | 50.46 | 51.38 | 51.48 | 51.61 | **52.01** | 51.79 | 51.11 |

**说明**: 5个AI发现架构(PG=PathGateFusionNet, C=ContentSharpRouter, FG=FusionGatedFIRNet, H=HierGateNet, AM=AdaMultiPathGateNet) vs 人类设计基线。最佳架构 FG 达到 57.28% 开发平均分，超越 DeltaNet +0.97分（近3倍于 Mamba2 的 +0.34）。

### Table 2: Data Curation Benchmarks / 数据整理基准

| Benchmark | Fineweb-Edu | Ultra-Fineweb | DCLM | Nemotron-CC | Nematron-CC ASI | Nematron-CC ASI+ |
|-----------|-------------|---------------|------|-------------|-----------------|------------------|
| BBH | 3.01 | 7.42 | 24.16 | **26.82** | 26.69 | 26.16 |
| ARC-E | 73.39 | 73.96 | 75.13 | 74.94 | 77.55 | **78.59** |
| ARC-C | 43.45 | 43.77 | 45.02 | 43.52 | 48.41 | **49.32** |
| MMLU | 28.38 | 25.53 | 28.54 | 27.49 | 32.55 | **46.13** |
| AGIEval | 16.96 | 17.72 | 17.90 | 18.15 | **18.30** | 18.21 |
| HellaSwag | 64.33 | 65.32 | **70.39** | 65.32 | 64.36 | 62.21 |
| TriviaQA | 0.67 | 0.42 | **42.85** | 25.33 | 26.96 | 26.65 |
| RACE | 35.43 | 34.28 | **36.08** | 35.04 | 35.63 | 34.28 |
| DROP | 6.78 | 7.78 | **24.31** | 19.57 | 19.48 | 18.49 |
| WinoGrande | 61.02 | 60.93 | **64.99** | 57.96 | 59.89 | 58.09 |
| PIQA | 75.80 | 75.63 | **77.93** | 76.79 | 76.80 | 76.15 |
| CSQA | 19.54 | 19.90 | 20.16 | 20.31 | 20.61 | **39.12** |
| SIQA | 45.02 | 43.43 | **47.51** | 44.36 | 43.58 | 43.57 |
| OpenBookQA | 39.92 | 39.84 | **43.36** | 39.80 | 42.20 | 41.44 |
| GPQA | 24.51 | 23.04 | 25.67 | 24.37 | 23.93 | **27.10** |
| MedQA | 26.36 | 24.84 | 24.88 | 26.77 | 26.11 | **40.25** |
| MedMCQA | 25.80 | 24.92 | 28.15 | 28.86 | 30.28 | **40.97** |
| PubMedQA | 67.04 | 64.44 | 66.56 | 67.68 | **68.32** | 67.68 |
| Avg | 36.52 | 36.29 | 42.42 | 40.17 | 41.20 | **44.13** |

**说明**: ASI-Evolve 发现的数据整理策略使 Nemotron-CC ASI+ 达到 44.13 平均分，超越原始数据 +3.96分。知识密集型任务提升显著：MMLU +18.64分，CSQA +18.80分，MedQA +13.48分。

### Table 3: RL Algorithm Benchmarks / 强化学习算法基准

| Method | Math500 | AMC32 | AIME25 | AIME24 | OlympiadBench |
|--------|---------|-------|--------|--------|---------------|
| GRPO (Human) | 82.0 | 67.5 | 20.00 | 20.00 | 45.92 |
| Algorithm 1 (ASI-Evolve) | **85.6** | **80.0** | 29.58 | **31.67** | **50.96** |
| Algorithm 2 | 84.6 | 77.5 | 23.75 | 23.33 | 48.74 |
| Algorithm 3 | 84.8 | 77.5 | **30.00** | 30.00 | 49.18 |
| Algorithm 4 | 82.4 | 75.0 | 20.00 | 20.00 | 46.81 |
| Algorithm 5 | 82.0 | 72.5 | 23.33 | 20.00 | 45.62 |

**说明**: ASI-Evolve 发现的RL算法在数学推理基准上显著超越 GRPO：AMC32 +12.5分（67.5→80.0），AIME24 +11.67分（20→31.67），OlympiadBench +5.04分。

### Table 4: Circle Packing Comparison / 圆填充任务对比

| Framework | Model | Rounds | Best Score |
|-----------|-------|--------|------------|
| AlphaEvolve | Gemini 2.0 Flash + Claude 3.7 | — | 2.6359 |
| OpenEvolve | Gemini 2.0 Flash + Claude 3.7 | 460 | 2.6343 |
| LoongFlow | DeepSeek-R1-250528 | — | 2.6360 |
| SkyDiscover | GPT-5 | 89 | 2.6360 |
| **ASI-Evolve (Ours)** | GPT-5-mini | **17** | **2.6360** |

**说明**: ASI-Evolve 仅需17轮达到SOTA级别2.636，远快于其他框架（OpenEvolve 460轮、SkyDiscover 89轮）。

### Table 5: Drug-Target Interaction Prediction / 药物-靶点相互作用预测

| Discovered by | Model | BindingDB-Dev AUROC | F1 | BindingDB-Random AUROC | F1 | Human AUROC | F1 | BioSNAP AUROC | F1 |
|---------------|-------|---------------------|----|-------------------------|----|-------------|----|---------------|----|
| Human | TransformerCPI | — | — | 93.96 | 87.02 | 96.03 | 91.35 | 86.35 | 78.53 |
| Human | PSICHIC | — | — | 91.67 | 83.30 | 98.55 | 94.85 | 91.64 | 84.87 |
| Human | ConPlex | — | — | 93.59 | 83.39 | 97.04 | 90.38 | 88.40 | 72.86 |
| Human | ColdStartCPI | — | — | 89.27 | 85.27 | 98.29 | 93.57 | 93.39 | 86.84 |
| Human | DrugBAN | 94.15 | 86.89 | 94.89 | 87.96 | 98.61 | 95.40 | 89.43 | 82.69 |
| **AI** | **ASI-Evolve** | **96.06** | **89.84** | **95.94** | **89.35** | **98.89** | **95.32** | 89.68 | 82.92 |

**说明**: ASI-Evolve 发现的DTI架构超越人类设计的 DrugBAN：BindingDB-Dev +1.91 AUROC，Human +0.28 AUROC。证明AI设计组件在真实生物医学任务中的实用价值。

### Table 6: Cold-Start DTI Performance / 冷启动DTI性能

| Discovered by | Model | Unseen Drug AUROC | F1 | Unseen Protein AUROC | F1 | Unseen Both AUROC | F1 |
|---------------|-------|-------------------|----|----------------------|----|--------------------|----|
| Human | DrugBAN | 79.15 | 77.39 | 82.26 | 75.76 | 76.47 | 71.53 |
| **AI** | **ASI-Evolve** | **86.09** | **82.35** | **85.82** | **78.44** | **80.83** | **74.51** |

**说明**: 冷启动场景下ASI-Evolve架构显著超越DrugBAN： unseen drug +6.94 AUROC，unseen protein +3.56，双重冷启动 +4.36。表明发现架构学到更鲁棒可迁移的分子交互表示。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Nemotron-CC | 672B tokens | 学术内容(数学/CS/医学/STEM) | 数据整理任务 |
| Skywork OR1 | — | 数学推理数据 | RL算法训练 |
| BindingDB | — | 药物-靶点相互作用 | DTI预测 |
| Circle Packing | 26 circles | 组合优化基准 | 框架对比 |

### 实现细节

**架构设计任务**:
- 探索阶段：~20M参数(8层，256隐藏)，2000步，1B tokens
- 验证阶段：~340M参数(24层，1024隐藏)，1B tokens
- 大规模验证：~1.3B参数(24层，2048隐藏)，100B tokens
- 评估：10核心基准 + 6 OOD测试集

**数据整理任务**:
- 模型：3B参数
- 训练：500B tokens
- 评估：18基准

**RL算法任务**:
- 探索：4B参数，150步
- 验证：14B参数，300步
- 基座：Qwen-3-14B-base

**Circle Packing消融**:
- 基座：GPT-5-mini (temp=0.7, top-p=0.95, max_tokens=32768)
- 或 Qwen3-32B (temp=0.6, top-p=0.95, thinking enabled)
- 采样：3节点，4并行worker，超时300s

### 关键发现

1. **架构设计**: 1773轮探索发现105个SOTA架构；51.7%源于认知库，44.8%（SOTA中）源于自身经验积累
2. **数据整理**: 系统收敛于清洗导向策略（噪声去除+格式归一化+领域感知保留），无预设指导
3. **RL算法**: 发现算法展示数学创新（成对优势估计、预算约束更新）
4. **跨领域迁移**: DTI架构引入 Sinkhorn Attention + Domain-Specific Marginalization + Top-k Sparse Gating

---

## 批判性思考

### 优点

1. **统一突破**: 首次在数据、架构、算法三组件展示AI驱动发现，覆盖完整AI研发栈
2. **方法论创新**: Cognition Base + Analyzer 设计解决长周期、弱监督研究的核心挑战
3. **实证强度**: 105个SOTA架构、MMLU +18分、AMC32 +12.5分等硬核数字
4. **跨领域验证**: DTI实验证明AI设计组件的实用价值超越AI/ML领域

### 局限性

1. **计算效率**: 架构设计在注意力层而非底层kernel，无法直接产出CUDA优化实现
2. **评估依赖**: fitness设计依赖sigmoid归一化+LLM Judge，可能引入主观偏差
3. **领域限制**: 认知库初始化依赖领域论文，全新领域需更长探索
4. **可复现性**: 大规模实验(GPU小时、数据规模)对研究资源要求高

### 潜在改进方向

1. **底层优化**: 结合kernel级演化，产出硬件优化实现
2. **多目标演化**: 扩展fitness处理 Pareto 最优权衡（性能 vs 效率 vs 可解释性）
3. **领域泛化**: 探索零认知启动模式，验证完全自主发现能力
4. **人机协作**: 研究人类科学家在闭环中的最佳介入点

### 可复现性评估

- [x] 代码开源 (GitHub: GAIR-NLP/ASI-Evolve)
- [ ] 预训练模型（文中未提及）
- [x] 训练细节完整（附录详述超参数）
- [x] 数据集可获取（Nematron-CC, Skywork OR1公开）

---

## 关联笔记

### 基于

- [[AlphaEvolve]]: 进化搜索范式基础
- [[FunSearch]]: 程序搜索先驱
- [[OpenEvolve]]: 开源进化框架对比
- [[DeltaNet]]: 架构设计任务基线
- [[GRPO]]: RL算法设计任务基线

### 对比

- [[MLE-bench]]: 结构化任务执行，目标预定义
- [[AI Scientist]]: 自动化论文管线而非前沿发现
- [[SciMaster]]: 科学问答，无实验执行

### 方法相关

- [[Evolutionary Agent]]: 核心范式
- [[Cognition Base]]: 关键创新组件
- [[LLM-as-a-Judge]]: 评估方法
- [[MAP-Elites]]: 质量-多样性采样
- [[UCB1 Sampling]]: 探索-利用平衡
- [[Linear Attention]]: 架构设计目标
- [[Neural Architecture Search]]: 任务类别

### 应用领域

- [[Drug-Target Interaction]]: 跨领域验证任务
- [[Pretraining Data Curation]]: 数据组件任务

---

## 速查卡片

> [!summary] ASI-Evolve: AI Accelerates AI
> - **核心**: AI-for-AI统一框架，learn–design–experiment–analyze闭环
> - **方法**: Cognition Base注入先验 + Analyzer提炼反馈 + Database积累经验
> - **结果**: 105个SOTA架构(+0.97分)，MMLU +18.64分，AMC32 +12.5分，DTI cold-start +6.94 AUROC
> - **代码**: https://github.com/GAIR-NLP/ASI-Evolve

---

*笔记创建时间: 2026-04-03*