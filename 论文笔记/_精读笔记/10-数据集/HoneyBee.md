---
title: "HoneyBee: Data Recipes for Vision-Language Reasoners"
method_name: "HoneyBee"
authors: [Hritik Bansal, Devandra Singh Sachan, Kai-Wei Chang, Aditya Grover, Gargi Ghosh, Wen-tau Yih, Ramakanth Pasunuru]
year: 2025
venue: arXiv
tags: [vl-reasoning, data-curation, chain-of-thought, synthetic-data, supervised-finetuning]
zotero_collection: null
image_source: online
arxiv_html: https://arxiv.org/html/2510.12225
created: 2025-04-13
---

# 论文笔记：HoneyBee: Data Recipes for Vision-Language Reasoners

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | FAIR at Meta, UCLA |
| 日期 | November 2025 |
| 项目主页 | 待补充 |
| 对比基线 | [[Math-LLaVA]], [[LLaVA-CoT]], [[R1-OneVision]] |
| 链接 | [arXiv](https://arxiv.org/abs/2510.12225) |

---

## 一句话总结

> 系统性地研究 VL 推理数据集构建策略，创建包含 2.5M CoT 样本的高质量数据集 HoneyBee，在多个基准上超越 SOTA 模型。

---

## 核心贡献

1. **系统化数据策略研究**: 对 context 来源、data interventions、scaling 三大数据构建维度进行全面 ablation，识别关键设计决策
2. **关键发现**: Caption-and-solve 和 text-only reasoning 数据增强带来显著提升；所有维度 scaling 都有收益
3. **大规模高质量数据集**: HoneyBee 包含 2.5M 样本，350K unique image-question pairs，超越现有 SOTA 模型

---

## 问题背景

### 要解决的问题
视觉语言模型（VLM）的推理能力训练依赖于高质量的 Chain-of-Thought（CoT）数据集，但现有数据集的构建原理缺乏系统性研究。具体问题包括：
- 不同 context（image-question pair）来源对性能的影响不明确
- 哪些数据增强策略真正有效未知
- 多模态场景下的 scaling 规律不清楚

### 现有方法的局限
1. **Math-LLaVA / LLaVA-CoT**: 使用各自的数据分布和 CoT 生成策略，难以对比效果来源
2. **缺乏公平对比**: 不同工作使用不同的训练算法、generator 模型，无法直接比较数据质量
3. **缺少 scaling 研究**: VL 推理数据的 scaling 应该沿着哪些轴（images/questions/CoTs）未知

### 本文的动机
通过控制实验条件（固定训练算法、generator 模型、评估协议），系统研究 VL 推理数据构建的三大维度，找出最优策略组合，构建高质量数据集。

---

## 方法详解

### 数据构建流程概览

HoneyBee 采用三阶段数据构建流程：

- **阶段一：Context Curation** — 评估不同 image-question 来源及其混合策略
- **阶段二：Data Interventions** — 应用针对性数据增强策略提升感知和问题解决能力
- **阶段三：Scaling** — 研究 images、questions、CoTs 三个维度的 scaling 效果

### Context Curation

#### Sourcing 策略

从多个现有 VL 推理数据集中选择 context（image, question）：

| 数据集 | 特点 | 来源类型 |
|--------|------|----------|
| ViRL | 整合多个 VL 数据集并清洗 | Expert-curated |
| Math-LLaVA | 几何、函数、表格等多场景 | Task-specific datasets |
| R1-OneVision | 竞赛级别数学问题 | Task-specific |
| ThinkLite-VL-Hard | 困难样本筛选 | Filtered |
| LLaVA-CoT | 通用 VL CoT 数据 | Mixed |
| MMK12 | K12 数学问题 | Education-focused |

**处理流程**:
1. 去重：使用 pHash 算法移除与评估数据集图像相同的样本
2. 控制规模：每个数据集取 50K 样本，聚焦数据质量而非数量
3. 生成 CoT：使用 Llama4-Scout (109B) 作为 generator
4. 过滤：可选地丢弃最终答案错误的 CoT

#### Mixing 策略

将 top-performing 数据源进行混合，测试 top-2、top-4、all datasets 的组合效果。

### Data Interventions

论文设计了两大类干预策略：

#### 1. Perception Enhancement（感知增强）

| 策略 | 类型 | 描述 |
|------|------|------|
| Visual Perturbation | Replacement/Augmentation | 对图像应用 rotation、distractor concatenation、mixup 变换 |
| Text-Rich Images | Replacement/Augmentation | 将问题文本嵌入图像，增强 OCR 能力 |
| Perceptual Redundancy | Filtering | 移除仅靠问题文本就能回答的样本 |
| Shallow Perception | Filtering | 移除仅靠 caption 就能回答的样本 |
| Caption-and-Solve | Augmentation | 在 CoT 前添加 image caption 作为辅助信号 |

#### 2. Problem-Solving Enhancement（问题解决增强）

| 策略 | 类型 | 描述 |
|------|------|------|
| Text-Only Reasoning | Augmentation | 添加高质量纯文本推理数据 |
| Increased Distractors | Replacement/Augmentation | 将选择题选项从 4 个增加到 10 个 |
| Length Filtering | Filtering | 只保留长 CoT 样本 |
| Difficulty Filtering | Filtering | 平衡各难度级别样本 |

### Scaling Diverse Data Axes

论文研究三个 scaling 维度：

1. **Scaling Images**: 增加唯一图像数量 $[\mathcal{D}_{N/8}, \mathcal{D}_{N/4}, \mathcal{D}_{N/2}, \mathcal{D}_{N}]$

2. **Scaling Questions**: 为每张图像合成多个问题
   - $Q^{\prime}_{j,i}=\mathcal{G}(I_{j},Q_{j})$ — 基于 (image, question) 生成新问题

3. **Scaling CoTs**: 为每个 (image, question) 生成多个 CoT 解法
   - $\mathcal{D}_{N}(n_{c}=k)=\{I_{j},Q_{j},\{C_{j,i}\}_{i=1}^{k}\}_{j=1}^{N}$

---

## 关键公式

### 公式1: VL 推理数据集定义

$$
\mathcal{D}=\{(I_{j},Q_{j},A_{j})\}_{j=1}^{N}
$$

**含义**: 源 VL 推理数据集包含 $N$ 个样本，每个样本包含图像 $I_j$、问题 $Q_j$ 和可选答案 $A_j$。

**符号说明**:
- $I_j$: 第 $j$ 个图像
- $Q_j$: 对应的问题
- $A_j$: 可选的最终答案

### 公式2: CoT 生成过程

$$
C_{j}=\mathcal{G}(I_{j},Q_{j})
$$

**含义**: 使用 generator 模型 $\mathcal{G}$ 为 (image, question) 对生成 Chain-of-Thought。

**符号说明**:
- $\mathcal{G}$: CoT generator 模型（本文使用 Llama4-Scout）
- $C_j$: 生成的 CoT，包含推理步骤 $S_j$ 和预测答案 $P_j$

### 公式3: CoT 结构

$$
C_{j}=[S_{j};P_{j}]
$$

**含义**: CoT 由推理步骤和最终答案拼接组成。

**符号说明**:
- $S_j$: 推理步骤（step-by-step solution, planning, verification）
- $P_j$: 预测的最终答案
- $;$: 文本空间拼接

### 公式4: 合成数据集定义

$$
\mathcal{D}_{\mathcal{G}}=\{(I_{j},Q_{j},C_{j},A_{j})\}_{j=1}^{N}
$$

**含义**: 合成数据集包含原始 context 和生成的 CoT。

### 公式5: VLM 训练目标

$$
\mathbb{E}_{(I_{j},Q_{j},C_{j})\in\mathcal{D}_{\mathcal{G}}}[\log p_{\theta}(C_{j}\mid I_{j},Q_{j})]
$$

**含义**: Supervised finetuning 目标，最大化在给定 context 下生成 CoT 的概率。

**符号说明**:
- $p_{\theta}$: 待训练的 VLM
- 条件生成：以图像和问题为条件生成 CoT

### 公式6: Caption-and-Solve 增强

$$
C_{j}^{\prime}=[I_{j}^{cap};C_{j}]
$$

**含义**: 在 CoT 前添加 image caption 作为辅助视觉信号。

**符号说明**:
- $I_{j}^{cap}$: 由 generator 生成的图像描述
- 该策略显著提升下游任务性能

### 公式7: Text-Only 数据增强

$$
\mathcal{D}_{\mathcal{G}}^{VL+Text}=\mathcal{D}_{\mathcal{G}}^{VL}\cup\mathcal{D}_{\mathcal{G}}^{Text}
$$

**含义**: 将纯文本推理数据与 VL 推理数据拼接，增强通用推理能力。

### 公式8: Perceptual Redundancy 过滤

$$
\hat{P_{j}}=\mathcal{G}(Q_{j}) \quad \text{if } \hat{P_{j}}=A_{j} \text{, remove}
$$

**含义**: 如果仅靠问题（不看图像）就能正确回答，则移除该样本，强制模型依赖视觉输入。

---

## 关键图表

### Figure 1: Summary of Results

![Figure 1a](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x1.png)

![Figure 1b](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x2.png)

**说明**: (a) 展示 HoneyBee 数据量增加带来的性能提升，涵盖 1B-8B 多个模型尺寸。(b) PLM-HoneyBee-3B 相比现有 VL CoT 数据集的相对增益。

### Figure 2: Data Curation Pipeline

![Figure 2](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x3.png)

**说明**: 三阶段数据构建流程：Context Curation → Data Interventions → Scaling → HoneyBee 数据集。

### Figure 3: Data Intervention Strategies

![Figure 3](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x4.png)

**说明**: 详细展示各类 data intervention 策略及其作用目标（感知 vs 问题解决）。

### Figure 4: Scaling Results

![Figure 4](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x5.png)

**说明**: 三种 scaling 轴的效果：(a) scaling images, (b) scaling questions, (c) scaling CoTs。所有维度 scaling 都带来收益。

### Figure 5: HoneyBee Statistics

![Figure 5](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x6.png)

**说明**: HoneyBee 数据集统计：2.5M 样本，350K unique questions，CoT 中包含多样化的推理行为关键词。

### Figure 6: Scaling Pipeline

![Figure 6](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x7.png)

**说明**: 从 context 数据集创建 1.5M VL reasoning 实例的 pipeline。

### Figure 7: Difficulty Analysis

![Figure 7](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x8.png)

**说明**: PLM-3B trained on HoneyBee 在各 MathVision difficulty level 上均超越 base model。

### Figure 8: Shared Caption Decoding

![Figure 8](https://ar5iv.labs.arxiv.org/html/2510.12225/assets/x9.png)

**说明**: 提出 Shared Caption Decoding 策略，在多次解题尝试中复用同一个 caption，减少 73% 推理 token 不牺牲准确率。

### Table 1: Performance Across Model Scales

| Model Scale | Method | Average | MathVerse | MathVista | MathVision | MMMU-Pro | We-Math | DynaMath | LogicVista | Hall. Bench | MATH500 | GPQA |
|-------------|--------|---------|-----------|-----------|------------|----------|---------|----------|------------|-------------|---------|------|
| **1B** | PLM-1B | 25.9 | 17.8 | 48.6 | 15.1 | 15.8 | 35.5 | 30.3 | 23.0 | 50.3 | 15.2 | 7.1 |
| | InternVL-2.5-1B | 27.6 | 18.7 | 44.2 | 16.4 | 16.2 | 38.7 | 29.3 | 20.5 | 51.6 | 27.8 | 12.1 |
| | InternVL-3-1B | 28.3 | 18.0 | 35.0 | 13.2 | 16.2 | 37.5 | 28.5 | 21.9 | 56.0 | 37.8 | 19.2 |
| | **PLM-HoneyBee-1B** | **36.2** | **29.4** | **53.7** | **23.0** | **18.8** | **50.6** | **39.3** | **28.6** | **56.1** | **36.8** | **25.8** |
| **3B-4B** | PLM-3B | 33.8 | 18.0 | 57.2 | 16.1 | 19.5 | 46.1 | 37.0 | 33.5 | 61.1 | 30.4 | 18.7 |
| | InternVL-2.5-4B | 41.5 | 28.7 | 61.8 | 24.7 | 31.1 | 55.6 | 40.7 | 32.1 | 65.5 | 49.4 | 25.3 |
| | Qwen2.5-VL-3B | 42.6 | 35.0 | 58.9 | 23.7 | 29.8 | 49.2 | 42.5 | 36.6 | 66.0 | 62.0 | 22.7 |
| | **PLM-HoneyBee-3B** | **46.2** | **42.8** | 61.2 | **29.9** | 28.4 | **59.3** | **51.9** | **36.6** | 65.0 | 59.4 | **27.7** |
| **7B-8B** | PLM-8B | 34.6 | 19.3 | 59.3 | 17.1 | 20.5 | 47.9 | 36.7 | 30.8 | 64.0 | 34.0 | 16.2 |
| | InternVL-2.5-8B | 41.4 | 27.3 | 61.5 | 21.4 | 32.0 | 56.6 | 41.9 | 26.6 | 63.8 | 57.0 | 26.3 |
| | InternVL-3-8B | 45.1 | 35.3 | 61.8 | 19.4 | 35.8 | 55.7 | 51.2 | 36.2 | 65.5 | 69.6 | 20.2 |
| | Qwen2.5-VL-7B | 48.5 | 42.0 | 67.5 | 27.6 | 37.1 | 61.1 | 51.3 | 39.9 | 67.4 | 64.8 | 26.3 |
| | **PLM-HoneyBee-8B** | **49.8** | **43.0** | **68.2** | 26.3 | 33.8 | **66.1** | **53.3** | **41.3** | **68.8** | 63.6 | **33.3** |

**说明**: PLM-HoneyBee 在所有模型尺寸上均达到最佳或接近最佳性能。带 * 标记为未参与数据构建的评估数据集。

### Table 2: Context Source Performance

| Context Sources | Models | Average | MathVerse | MathVista | MathVision | MMMU-Pro | We-Math |
|-----------------|--------|---------|-----------|-----------|------------|----------|---------|
| ViRL | Avg. | **40.1** | **33.7** | **61.6** | 24.5 | **26.6** | **53.9** |
| Math-LLaVA | Avg. | 37.7 | 31.8 | 59.2 | 23.5 | 23.9 | 50.3 |
| R1-OneVision | Avg. | 37.3 | 31.6 | 57.9 | 21.1 | 24.9 | 51.0 |
| ThinkLite-VL-Hard | Avg. | 37.1 | 28.4 | 57.5 | **24.7** | 24.2 | 50.6 |
| LLaVA-CoT | Avg. | 36.2 | 30.6 | 57.4 | 18.8 | 24.5 | 50.0 |
| MMK12 | Avg. | 36.0 | 26.0 | 56.0 | 19.9 | 23.2 | 49.0 |

**关键发现**: ViRL 作为 context source 表现最佳，平均性能达 40.1%，比最差的 MMK12 高 11.4pp。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| MathVerse (testmini) | 788 | 几何与函数问题，vision-only subset | 验证 |
| MathVista (testmini) | 1000 | 多场景视觉推理（表格、函数、几何） | 验证 |
| MathVision (testmini) | 304 | 竞赛级别数学问题 | 验证 |
| MMMU-Pro (vision) | 1730 | 多学科专业问题 | 验证 |
| We-Math (testmini) | 1740 | 多知识粒度 | 验证 |
| DynaMath | 1000 | 动态数学问题 | 额外评估 |
| LogicVista | 448 | 逻辑推理 | 额外评估 |
| HallusionBench | - | 视觉感知挑战 | 额外评估 |
| MATH500 | 500 | 纯文本数学推理 | 额外评估 |
| GPQA (diamond) | 198 | 科学推理 | 额外评估 |

### 实现细节

- **Generator Model**: Llama4-Scout (109B total, 17B active)
- **Base Models**: PLM-3B / PLM-8B (Perception Language Models)
- **训练轮数**: 5 epochs，选择最佳 checkpoint
- **数据规模**: 数据 curation 实验中限制 50K 样本
- **评估**: 使用 greedy decoding，最大生成长度 2048

### 主要实验结果

1. **Context Source 影响**: 不同来源导致最高 11.4pp 性能差异
2. **Data Interventions**: Caption-and-Solve 和 Text-Only Reasoning 带来最大增益
3. **Scaling**: 所有三个维度（images/questions/CoTs）scaling 都有收益
4. **最终性能**: PLM-HoneyBee-3B 比 Qwen2.5-VL-3B 高 8.4pp；比 base model 高 24.8pp

---

## 批判性思考

### 优点
1. **系统性研究**: 首次对 VL 推理数据构建进行全面 ablation
2. **公平对比**: 固定训练算法、generator、评估协议，确保结论可靠
3. **实用价值**: 识别出 caption-and-solve 和 text-only reasoning 两个高效策略
4. **资源贡献**: 发布大规模高质量数据集

### 局限性
1. **Generator 固定**: 仅使用 Llama4-Scout，结论可能不通用
2. **任务聚焦**: 主要研究数学推理，其他 VL 任务可能不同
3. **单图像限制**: 未研究多图像推理场景
4. **Generator-to-Student**: 使用强 generator 教弱 student，self-improvement 路径未探索

### 潜在改进方向
1. 扩展到通用 VL 任务（VQA 等）
2. 研究多图像推理数据构建
3. 探索不同 generator 模型的效果
4. 结合 RL 训练进一步提升

### 可复现性评估
- [ ] 代码开源（待补充）
- [ ] 数据集公开
- [ ] 训练细节完整
- [ ] 评估数据集可获取

---

## 关联笔记

### 基于
- [[OpenThoughts]]: Text-only reasoning 数据构建参考
- [[STaR]]: Self-Taught Reasoner 方法基础

### 对比
- [[Math-LLaVA]]: Context source 对比
- [[LLaVA-CoT]]: Context source 对比
- [[R1-OneVision]]: Context source 对比

### 方法相关
- [[Chain-of-Thought]]: 核心推理范式
- [[Supervised Finetuning]]: 训练方法
- [[Knowledge Distillation]]: Generator-to-Student 范式
- [[Test-Time Scaling]]: 推理时计算利用
- [[Self-Consistency]]: 多解法聚合

### 技术相关
- [[VLM]]: 视觉语言模型基础
- [[Vision-Language Reasoning]]: 任务类型

---

## 速查卡片

> [!summary] HoneyBee: Data Recipes for Vision-Language Reasoners
> - **核心**: VL 推理数据构建的系统性研究
> - **方法**: Context Curation + Data Interventions + Scaling 三阶段流程
> - **数据**: 2.5M 样本，350K unique questions
> - **结果**: PLM-HoneyBee-3B 比 SOTA 高 8.4pp，比 base 高 24.8pp
> - **关键发现**: Caption-and-Solve、Text-Only Reasoning、全维度 Scaling 均有效

---

*笔记创建时间: 2025-04-13*