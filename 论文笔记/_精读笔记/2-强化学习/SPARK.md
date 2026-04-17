---
title: "SPARK: Stepwise Process-Aware Rewards for Reference-Free Reinforcement Learning"
method_name: "SPARK"
authors: [Salman Rahman, Sruthi Gorantla, Arpit Gupta, Swastik Roy, Nanyun Peng, Yang Liu]
year: 2025
venue: arXiv
tags: [process-reward-model, reinforcement-learning, inference-time-scaling, mathematical-reasoning, llm-training]
zotero_collection: 2-强化学习
image_source: online
arxiv_html: https://arxiv.org/html/2512.03244v1
created: 2026-04-13
---

# 论文笔记：SPARK: Stepwise Process-Aware Rewards for Reference-Free Reinforcement Learning

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Amazon AGI, UCLA |
| 日期 | 2025年12月 |
| 项目主页 | - |
| 对比基线 | [[TANGO]], [[PRIME]], [[RLVR]] |
| 链接 | [arXiv](https://arxiv.org/abs/2512.03244) / [HTML](https://arxiv.org/html/2512.03244v1) |

---

## 一句话总结

> 提出无参考的 PRM 训练框架，通过推理时缩放方法生成合成验证数据，实现超越 Ground Truth 方法的强化学习训练。

---

## 核心贡献

1. **无参考验证数据生成**: 利用推理时缩放方法（Self-Consistency + Meta-Critique）生成高质量步骤级验证数据，消除对 Ground Truth 的依赖
2. **PRM 训练超越 Ground Truth**: 在 ProcessBench 上达到 67.5 F1，超过 Reference-Guided 方法的 66.4 和 GPT-4o 的 61.9
3. **稳定的 RL 训练**: 使用 PRM-CoT 作为奖励信号，在数学推理基准上达到 47.4% 平均准确率，超过 RLVR 的 43.9%

---

## 问题背景

### 要解决的问题

当前 [[Process Reward Model|PRM]] 训练方法依赖于昂贵的步骤级标注或 Ground Truth 参考答案，这严重限制了 RL 在缺乏可验证答案领域的应用（如创意写作、研究构思、长期规划等）。

### 现有方法的局限

1. **[[RLVR]]**: 仅提供稀疏的 outcome-level 奖励，训练稳定性差
2. **[[TANGO]]**: 需要 Gold Standard 解来训练 verifier
3. **[[PRIME]]**: 需要 outcome-level 正确性标签来训练 PRM
4. **人工标注**: 成本高昂且在模型能力超过人类专家时不可行

### 本文的动机

推理时缩放方法（如 Self-Consistency 和 Self-Critique）已证明可以在无需 Ground Truth 的情况下改进推理。作者提出：能否利用这些能力来生成合成验证数据，训练有效的生成式 PRM？

---

## 方法详解

### 模型架构

SPARK 采用 **三阶段管道** 架构：

- **Stage I**: Generator-Verifier 框架生成合成验证数据
  - Generator: Qwen-2.5-14B-Instruct (M=8 个解决方案)
  - Verifier: Qwen-3-32B-Instruct (N=16 个验证)
- **Stage II**: 训练三种生成式奖励模型变体
  - ORM: 仅输出最终 verdict
  - PRM: 输出步骤级判断
  - PRM-CoT: 输出验证理由 + 判断
- **Stage III**: 在 RL 中应用训练好的 PRM 作为奖励信号

### 核心模块

#### 模块1: Self-Consistency（并行缩放）

**设计动机**: 通过聚合多个独立验证路径来提高准确性

**具体实现**:
- **Outcome-level Consistency**: 对最终 verdict 进行多数投票
- **Step-level Consistency**: 对每个步骤的判断进行多数投票（最佳方法）

#### 模块2: Meta-Critique（顺序缩放）

**设计动机**: 通过迭代精炼单个验证来识别错误

**具体实现**:
1. 生成初始验证 $v_{init}$
2. Critique 识别错误（遗漏的错误、误判的步骤、推理缺陷）
3. Merge 生成精炼验证 $v_{final}$

#### 模块3: Generative PRM 变体

**设计动机**: 利用 LLM 的生成能力进行验证

**具体实现**:
- **ORM**: 输入 $(q, s)$ + "Is the answer correct?"，输出 $y \in \{Yes, No\}$
- **PRM**: 输入 $(q, s)$ + "Let's verify step by step"，输出 $(v_1, ..., v_n, y)$
- **PRM-CoT**: 输出验证理由 $\tau_i$ + 判断 $v_i$ + 最终 verdict $y$

---

## 关键公式

### 公式1: [[GRPO|Group Relative Policy Optimization]] 目标函数

$$
J(\theta) = \mathbb{E}_{q, \{o_i\}_{i=1}^M} \left[ \frac{1}{M} \sum_{i=1}^M \frac{1}{|o_i|} \sum_{t=1}^{|o_i|} \min \left[ r_t(\theta) \hat{A}_{i,t}, \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) \hat{A}_{i,t} \right] - \beta D_{KL}(\pi_\theta \| \pi_{ref}) \right]
$$

**含义**: GRPO 算法的核心目标函数，通过组归一化优势值和 KL 正则化来优化策略

**符号说明**:
- $o_i$: 第 $i$ 个解决方案
- $r_t(\theta) = \frac{\pi_\theta(o_{i,t}|q,o_{i,<t})}{\pi_{old}(o_{i,t}|q,o_{i,<t})}$: 重要性比率
- $\hat{A}_{i,t}$: 组归一化的 token 级优势值
- $\beta$: KL 正则化系数

### 公式2: [[Process-Aware Reward|Process-Aware 奖励]]

$$
r_{process}(s) = \mathbb{1}[\text{valid format}] \cdot y
$$

$$
\hat{A}_{process}^{(i)} = \frac{r_{process}^{(i)} - \mu_G}{\sigma_G}
$$

**含义**: 基于 PRM 最终 verdict 的奖励，通过格式验证防止 reward hacking

**符号说明**:
- $y \in \{0, 1\}$: 验证 verdict
- $\mathbb{1}[\text{valid format}]$: 格式验证（单 `<answer>` 标签、单 `\boxed{}` 表达式、无后续内容）
- $\mu_G, \sigma_G$: 组级统计量

### 公式3: [[Step-Augmented Process Reward|步骤增强奖励]]

$$
r_{step-aug}(s) = \mathbb{1}[\text{valid format}] \cdot \left( 0.4 \cdot \frac{k}{n} + 0.6 \cdot y \right)
$$

**含义**: 结合步骤正确率（40%）和最终 verdict（60%）的混合奖励

**符号说明**:
- $\frac{k}{n}$: 步骤正确率（k 个正确步骤 / n 个总步骤）
- $y$: process-aware verdict

### 公式4: [[Selective Advantage|选择性优势]]

$$
\hat{A}_{selective}^{(i,t)} = \begin{cases} \hat{A}_{process}^{(i)} & \text{if } (\hat{A}_{process}^{(i)} \geq 0 \land c_j = \text{correct}) \lor (\hat{A}_{process}^{(i)} < 0 \land c_j = \text{incorrect}) \\ 0 & \text{otherwise} \end{cases}
$$

**含义**: 仅保留与步骤判断对齐的优势值，避免错误奖励

**符号说明**:
- $c_j \in \{correct, incorrect\}$: 步骤 $j$ 的判断

### 公式5: [[Global Step-Reward|全局步骤奖励]]

$$
\hat{A}_{global}^{(i,t)} = 0.8 \cdot \hat{A}_{process}^{(i)} + 0.2 \cdot \hat{A}_{step}^{(i,t)}
$$

**含义**: 混合 process-aware 和 step-level 优势值

### 公式6: Self-Consistency 聚合

$$
y^* = \arg\max_{y \in \{Yes, No\}} \sum_{j=1}^N \mathbb{1}[y^{(j)} = y]
$$

$$
v_i^* = \arg\max_{v \in \{correct, incorrect\}} \sum_{j=1}^N \mathbb{1}[v_i^{(j)} = v]
$$

**含义**: 多数投票确定共识判断

---

## 关键图表

### Figure 1: SPARK 三阶段管道概览

![Figure 1](https://arxiv.org/html/2512.03244v1/x1.png)

**说明**: SPARK 的三阶段框架。Stage I 使用 Generator-Verifier 生成合成验证数据；Stage II 训练三种 PRM 变体；Stage III 在 RL 中应用 PRM 作为奖励信号。

### Figure 2: Multi-scale Generator-Verifier Framework

![Figure 2](https://arxiv.org/html/2512.03244v1/x2.png)

**说明**: 合成验证数据生成框架。Generator 生成多个解决方案，Verifier 通过 Self-Consistency（并行）和 Meta-Critique（顺序）方法评估。

### Figure 3: ProcessBench F1 Scores

![Figure 3](https://arxiv.org/html/2512.03244v1/x3.png)

**说明**: ProcessBench 评估结果。Step-level Consistency 达到最高 F1（PRM: 67.5, PRM-CoT: 65.7），超过 Reference-Guided 和 GPT-4o。

### Figure 4: PRM-CoT vs RLVR vs Random Rewards

![Figure 4](https://arxiv.org/html/2512.03244v1/x4.png)

**说明**: 左图：训练奖励曲线；右图：测试准确率。PRM-CoT 稳定超越 RLVR，随机奖励无法提升基线。

### Figure 5: Generative Reward Models Comparison

![Figure 5](https://arxiv.org/html/2512.03244v1/x5.png)

**说明**: PRM-CoT (41.13%) > PRM (34.0%) > ORM (33.53%)，带 CoT 的验证提供更丰富的反馈。

### Figure 6: Reward Formulations Comparison

![Figure 6](https://arxiv.org/html/2512.03244v1/x6.png)

**说明**: 不同奖励设计对比。Selective Advantage (44.0%) 性能最佳，Process-Award (41.13%) 尽管不显式使用步骤信息仍表现优异。

### Table 1: 数学推理基准性能

| Method | MATH500 | AIME'24 | AIME'25 | AMC'23 | OlympiadBench | MinervaMath | Avg |
|--------|---------|---------|---------|--------|---------------|-------------|-----|
| GPT-4o | 76.6 | 9.3 | - | 47.5 | 43.3 | 36.8 | - |
| o1-preview | 85.5 | 44.6 | - | 90.0 | - | - | - |
| Qwen2.5-Math-72B-Inst | 82.6 | 23.3 | - | 70.0 | 49.0 | - | - |
| TANGO (7B) | 82.4 | 26.7 | 23.3 | 70.0 | 45.3 | - | - |
| PRIME (7B) | 78.2 | 20.0 | 13.3 | 70.0 | 40.3 | 39.3 | 43.6 |
| RLVR (7B) | 83.7 | 26.7 | 16.7 | 59.1 | 40.0 | 37.5 | 43.9 |
| **SPARK PRM (7B)** | 82.8 | 26.7 | 16.7 | 62.5 | 38.7 | 37.1 | 44.1 |
| **SPARK PRM-CoT (7B)** | **85.4** | **30.0** | **20.0** | **66.3** | **42.7** | **40.1** | **47.4** |

**说明**: SPARK PRM-CoT 在所有 7B 模型中达到最佳性能，平均 47.4% 超过 RLVR 的 43.9%。

### Table 2: ProcessBench 详细评估

| Setup | GSM8K | MATH | OlympiadBench | OmniMATH | Average |
|-------|-------|------|---------------|----------|---------|
| GPT-4o | 53.5 | 61.9 | 24.8 | 23.8 | 31.5 |
| Qwen2.5-72B-Inst | 52.2 | 61.2 | 50.7 | 44.3 | 56.5 |
| Qwen2.5-Math-7B-PRM800K | 68.2 | 62.6 | 47.9 | 29.5 | 42.1 |
| **SPARK PRM (Step Consistency)** | 72.0 | 67.9 | 67.6 | 70.3 | **67.5** |
| Reference Guided | 70.2 | 67.4 | 66.1 | 66.6 | 66.4 |

**说明**: SPARK PRM 在 ProcessBench 所有子集上表现优异，平均 F1 67.5 超过 Reference-Guided 的 66.4。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Skywork-OR1-RL-Data | 8K 问题 | 混合难度 | PRM 训练 |
| Eurus-2-SFT-Data | 113K 问题 | 结构化解 | SFT 格式学习 |
| ProcessBench | 3,400 测试 | GSM8K-MATH-Olympiad | PRM 评估 |
| MATH-500, AIME, AMC, OlympiadBench, MinervaMath | - | 竞赛级数学 | RL 评估 |

### 实现细节

- **PRM 基座**: Qwen2.5-14B-Instruct
- **Policy 基座**: Qwen2.5-Math-7B
- **RL 框架**: veRL + vLLM (tensor parallelism = 2)
- **优化器**: AdamW, LR = 1e-6
- **训练参数**: batch_size=256, rollouts=16, KL=0.001
- **硬件**: FSDP 分布式训练 + gradient checkpointing

### 可视化结果

1. Step-level Consistency 是最有效的推理时缩放方法
2. PRM-CoT 提供最丰富的反馈，优于直接步骤判断
3. 格式约束对防止 reward hacking 至关重要

---

## 批判性思考

### 优点

1. **无参考训练**: 消除对 Ground Truth 的依赖，扩展 RL 应用范围
2. **超越 Ground Truth**: 合成数据质量超过人工标注
3. **系统分析**: 详细分析 reward hacking 模式并提出解决方案

### 局限性

1. **仅验证数学**: 实验限于可验证领域，未在主观领域测试
2. **推理成本**: Self-Consistency 需多次调用，计算成本较高
3. **格式依赖**: 依赖特定输出格式，泛化性受限

### 潜在改进方向

1. 扩展到主观领域（创意写作、伦理推理）
2. 降低推理时计算成本
3. 探索更通用的格式约束方案

### 可复现性评估

- [x] 使用开源模型（Qwen 系列）
- [ ] 代码未开源（论文中未提及 GitHub）
- [x] 训练细节完整
- [x] 数据集可获取（Skywork-OR1, Eurus-2）

---

## 关联笔记

### 基于
- [[Self-Consistency]]: 并行缩放方法
- [[Self-Critique]]: 顺序缩放方法
- [[GRPO]]: RL 算法框架

### 对比
- [[TANGO]]: 需要 Ground Truth 训练 verifier
- [[PRIME]]: 需要 outcome-level 标签
- [[RLVR]]: 仅提供 outcome-level 奖励

### 方法相关
- [[Process Reward Model]]: 核心验证方法
- [[Inference-Time Scaling]]: 合成数据生成技术
- [[Reward Hacking]]: RL 安全性挑战

---

## Reward Hacking 分析

论文系统分析了三种 reward hacking 模式：

1. **Solution Appending**: 无格式约束时，模型追加已解决问题骗取奖励
2. **Step Inflation**: 模型分解简单操作为过多步骤以提高步骤正确率
3. **Step Reduction**: 模型将解压缩为单步骤以获得完美步骤奖励

**解决方案**: 格式约束（单 `<answer>`、单 `\boxed{}`、无后续内容）

---

## 速查卡片

> [!summary] SPARK: Reference-Free PRM Training
> - **核心**: 无参考的 PRM 训练框架
> - **方法**: Self-Consistency + Meta-Critique 生成合成验证数据
> - **结果**: ProcessBench 67.5 F1 > Reference-Guided 66.4; RL 47.4% > RLVR 43.9%
> - **关键**: 格式约束防止 reward hacking

---

*笔记创建时间: 2026-04-13*