---
title: "GLM-5: from Vibe Coding to Agentic Engineering"
method_name: "GLM-5"
authors: [GLM-5 Team]
year: 2026
venue: arXiv
tags: [large-language-model, mixture-of-experts, reinforcement-learning, agentic-ai, coding-agent, long-context, sparse-attention]
zotero_collection: 11-深度学习基础
image_source: mixed
arxiv_html: https://arxiv.org/html/2602.15763
created: 2026-04-17
---

# 论文笔记：GLM-5: from Vibe Coding to Agentic Engineering

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Zhipu AI & Tsinghua University |
| 日期 | February 2026 |
| 项目主页 | https://github.com/zai-org/GLM-5 |
| 对比基线 | [[DeepSeek-V3]], [[Claude Opus 4.5]], [[Gemini 3 Pro]], [[GPT-5.2]] |
| 链接 | [arXiv](https://arxiv.org/abs/2602.15763) / [Code](https://github.com/zai-org/GLM-5) |

---

## 一句话总结

> GLM-5 是智谱 AI 发布的下一代大模型，采用 744B 参数 MoE + DSA 架构，通过异步强化学习实现从"vibe coding"到"agentic engineering"的范式转变，在真实软件工程任务上达到前沿水平。

---

## 核心贡献

1. **DSA 架构创新**: 采用 [[DeepSeek Sparse Attention|DSA]] 实现 128K 长上下文的 1.5-2x 计算效率提升，保持无损性能
2. **异步 Agent RL 系统**: 提出完全异步解耦的 RL 框架，解决长 horizon agent 任务训练效率问题
3. **多阶段 RL Pipeline**: 设计 Reasoning RL → Agentic RL → General RL + On-policy Cross-Stage Distillation 的渐进式训练流程
4. **国产芯片全栈适配**: 在华为 Ascend 等 7 个国产芯片平台完成深度优化部署

---

## 问题背景

### 要解决的问题

将大模型从"vibe coding"（人类提示词编程）范式转变为"agentic engineering"（AI 智能体自主工程），解决：
- 长上下文推理的计算成本问题
- Agent RL 训练的 GPU 利用率瓶颈
- 多阶段 RL 的能力遗忘问题
- 真实软件工程任务的端到端完成能力

### 现有方法的局限

1. 传统 dense attention 在 128K+ 长上下文时 O(L²) 计算成本过高
2. 同步 RL 在 agent rollout 时存在严重 GPU idle time
3. 多阶段 RL 会导致前期阶段能力退化
4. SWE-bench 等静态 benchmark 无法反映真实工程场景

### 本文的动机

作者认为通过：
- DSA 动态稀疏注意力可在不损失性能的情况下大幅降低长上下文成本
- 异步 RL 解耦训练与生成引擎可提升 GPU 利用率
- On-policy Cross-Stage Distillation 可恢复各阶段能力
- CC-Bench-V2 真实工程 benchmark 可评估端到端能力

---

## 方法详解

### 模型架构

GLM-5 采用 **[[Mixture of Experts|MoE]] + [[DeepSeek Sparse Attention|DSA]]** 架构：

- **总参数**: 744B（激活 40B）
- **架构层数**: 80 层（3 dense + 75 MoE + 1 MTP）
- **专家数**: 256 专家，路由 8 + 共享 1
- **注意力**: [[Multi-latent Attention|MLA]] + DSA Indexer
- **上下文长度**: 200K tokens
- **词汇表**: 154,880

### 核心模块

#### 模块1: [[Multi-latent Attention|MLA]]

**设计动机**: 减少 KV cache 内存占用，提升长上下文推理效率

**具体实现**:
- 使用 576 维 latent KV cache 替代传统 KV
- 提出 **Muon Split** 优化器配方：对每个 attention head 独立应用矩阵正交化
- 头维度从 192 增至 256，减少解码计算量
- 性能匹配 GQA-8，同时节省 GPU 内存

#### 模块2: [[DeepSeek Sparse Attention|DSA]]

**设计动机**: 动态稀疏注意力，根据 token 重要性选择性计算

**具体实现**:
- 通过 Lightning Indexer 检索 top-k 最相关 KV entries
- 只在检索到的子集上计算 sparse attention
- 训练策略：dense warm-up (1000 steps) + sparse adaptation (20B tokens)
- 长上下文计算成本降低 1.5-2x，性能无损

#### 模块3: [[Multi-token Prediction|MTP]]

**设计动机**: 提升基础模型性能 + 作为 speculative decoding draft model

**具体实现**:
- 训练时共享 3 个 MTP 层参数
- 推理时预测 next 2 tokens
- Accept length: 2.76（vs DeepSeek-V3.2 的 2.55）
- 减少 training-inference discrepancy

#### 模块4: [[GRPO|GRPO + IcePop]] RL Backbone

**设计动机**: 解决 MoE 模型 RL 训练的 training-inference mismatch

**具体实现**:
- 区分 training policy $\pi_{train}$ 和 inference policy $\pi_{infer}$
- 使用 mismatch ratio $\rho_{i,t}$ 做重要性采样校正
- 移除 KL regularization 加速 RL improvement
- 使用 pop(·) operator 截断极端 mismatch samples

---

## 关键公式

### 公式1: [[GRPO|GRPO + IcePop Loss]]

$$
L(\theta) = -\mathbb{E}_{x \sim D, \{y_i\}_{i=1}^G \sim \pi_{\theta_{old}}^{infer}} \left[ \frac{1}{G} \sum_{i=1}^G \frac{1}{|y_i|} \sum_{t=1}^{|y_i|} \text{pop}(\rho_{i,t}, 1/\beta, \beta) \cdot \min \left( r_{i,t} \hat{A}_{i,t}, \text{clip}(r_{i,t}, 1-\epsilon_{low}, 1+\epsilon_{high}) \hat{A}_{i,t} \right) \right]
$$

**含义**: GRPO 的改进版本，使用 IcePop 技术处理 training-inference mismatch

**符号说明**:
- $\rho_{i,t} = \frac{\pi_{\theta_{old}}^{train}(y_{i,t}|x, y_{i,<t})}{\pi_{\theta_{old}}^{infer}(y_{i,t}|x, y_{i,<t})}$: training-inference mismatch ratio
- $\text{pop}(\rho, 1/\beta, \beta)$: 截断极端 mismatch 的 operator
- $\hat{A}_{i,t}$: group-normalized advantage
- $\beta = 2, \epsilon_{low} = 0.2, \epsilon_{high} = 0.28$

### 公式2: [[On-policy Cross-Stage Distillation|Cross-Stage Distillation]]

$$
\hat{A}_{i,t} = \text{sg} \log \frac{\pi_{teacher}^{infer}(y_{i,t}|x, y_{i,<t})}{\pi_{train}(y_{i,t}|x, y_{i,<t})}
$$

**含义**: 用 teacher model logits 替代 advantage，恢复前期阶段能力

**符号说明**:
- $\text{sg}$: stop gradient (如 `.detach()`)
- $\pi_{teacher}^{infer}$: 前期阶段的 teacher model
- 用于多阶段 RL 后的 final refinement

### 公式3: [[Asynchronous Agent RL|异步 Agent RL 优化目标]]

$$
L(\theta) = \mathbb{E}_t \left[ f(r_t(\theta), \epsilon_l, \epsilon_h) \hat{A}_t \log \pi_\theta(a_t|s_t) \right]
$$

**含义**: 异步 RL 的 token-level clipping 策略，避免历史 policy tracking

**符号说明**:
- $r_t(\theta) = \exp(\log \pi_\theta(a_t|s_t) - \log \pi_{rollout}(a_t|s_t))$: importance sampling ratio
- $f(x; \epsilon_\ell, \epsilon_h)$: calibration function，超出区间则 mask

### 公式4: Calibration Function

$$
f(x; \epsilon_\ell, \epsilon_h) = \begin{cases} x, & \text{if } 1-\epsilon_\ell < x < 1+\epsilon_h \\ 0, & \text{otherwise} \end{cases}
$$

**含义**: 双侧截断策略，防止极端 policy divergence

---

## 关键图表

### Figure 1: ARC Benchmark Results

![[GLM-5_fig1.png|600]]

**说明**: GLM-5 在 8 个 agentic/reasoning/coding benchmark 上的表现，平均比 GLM-4.7 提升约 20%，与 Claude Opus 4.5 和 GPT-5.2 (xhigh) 相当。

### Figure 2: Intelligence Index v4.0

![[GLM-5_fig2.png|600]]

**说明**: GLM-5 在 Artificial Analysis Intelligence Index v4.0 上得分 50，成为首个达到此分数的开源权重模型。

### Figure 3: LMArena Results

![[GLM-5_fig3.png|600]]

**说明**: GLM-5 在 LMArena Text Arena 和 Code Arena 均为 #1 开源模型。

### Figure 4: Long-horizon Tasks

![[GLM-5_fig4.png|600]]

**说明**: Vending-Bench 2（左）和 CC-Bench-V2（右）结果，GLM-5 在长期规划任务上表现优异。

### Figure 5: Training Pipeline

![[GLM-5_fig5.png|600]]

**说明**: GLM-5 的完整训练流程：Base Model (27T tokens) → Mid-training (32K→128K→200K) → Post-training (SFT → Reasoning RL → Agentic RL → General RL → Cross-Stage Distillation)。

### Figure 6: MLA vs DSA SFT Loss

![[GLM-5_fig6.png|600]]

**说明**: MLA 和 DSA 模型在 SFT 训练时的 loss 曲线对比，两者基本持平。

### Figure 7: Thinking Modes

![[GLM-5_fig7.png|600]]

**说明**: GLM-5 支持的三种 thinking 模式：Interleaved Thinking（每个 response/tool call 前思考）、Preserved Thinking（保留跨轮次思考）、Turn-level Thinking（按轮次控制）。

### Figure 8: Context Management Strategies

![[GLM-5_fig8.png|600]]

**说明**: BrowseComp 上不同 context management 策略效果，Hierarchical Context Management (HCM) 达到 75.9%。

### Figure 9: Reward Hacking Examples

![[GLM-5_fig9.png|600]]

**说明**: Slides RL 训练中的 reward hacking 行为示例：硬截断过长内容、过度调整间距。

### Figure 10: Agent-as-a-Judge Pipeline

![[GLM-5_fig10.png|600]]

**说明**: Frontend evaluation 的自动化 pipeline：Build → Agent-as-a-Judge（GUI agent 交互验证）。

### Figure 11: Real-world General Abilities

![[GLM-5_fig11.png|600]]

**说明**: GLM-5 vs GLM-4.7 在五个真实场景能力的对比：翻译、多语言对话、指令跟随、世界知识、工具调用。

### Table 1: GQA-8 vs MLA Evaluation

| Dataset | GQA-8 | MLA | MLA + Muon Split | MLA-256 + Muon Split |
|---------|-------|-----|------------------|----------------------|
| Hellaswag | 77.3 | 77.3 | 77.8 | 77.4 |
| MMLU | 61.2 | 61.5 | 62.5 | 62.0 |
| C-Eval | 60.0 | 59.7 | 62.1 | 59.9 |
| RACE | 79.6 | 77.8 | 79.9 | 79.6 |
| BBH | 53.3 | 48.9 | 51.8 | 51.3 |
| GSM8K | 47.6 | 46.2 | 45.0 | 47.5 |
| HumanEval | 38.5 | 33.5 | 36.7 | 36.6 |

**说明**: Muon Split 使 MLA 性能匹配 GQA-8。

### Table 2: MTP Accept Length

| Model | Accept Length |
|-------|---------------|
| DeepSeek-V3.2 | 2.55 |
| GLM-5 | 2.76 |

**说明**: GLM-5 的 MTP accept length 更长，speculative decoding 效率更高。

### Table 3: MLA vs DSA Long-context Benchmarks

| Benchmark | MLA | DSA |
|-----------|-----|-----|
| MQ-NIAH-128k | 100.0 | 100.0 |
| MV-NIAH-128k | 95.5 | 97.0 |
| SQuAD-128k | 79.7 | 86.0 |
| HotpotQA-128k | 66.3 | 63.0 |

**说明**: DSA 长上下文性能与 MLA 相当。

### Table 4: SWA Pattern Results

| Model | 4K | 8K | 16K | 32K | 64K | 128K |
|-------|-----|-----|------|------|------|-------|
| GLM-9B (Full Attn) | 95.19 | 93.67 | 92.01 | 91.09 | 85.35 | 75.28 |
| SWA Interleave | 94.87 | 54.02 | 25.89 | 12.61 | 8.32 | 6.51 |
| SWA Pattern | 95.78 | 92.54 | 88.92 | 82.52 | 70.23 | 53.95 |

**说明**: Search-based SWA pattern 远优于固定 interleaved 模式。

### Table 5: Long-context Benchmark Comparison

| Method | RULER (64K/128K) | MRCR (64K/128K) |
|--------|------------------|-----------------|
| GLM-9B | 85.35/75.28 | 36.53/35.39 |
| SWA Interleave | 65.94/44.93 (↓19.41/↓30.35) | 30.03/28.83 |
| SWA Pattern | 83.72/69.59 (↓1.63/↓5.69) | 35.02/33.58 |
| GDN | 76.76/64.00 | 31.72/30.22 |
| SimpleGDN | 81.76/67.03 | 33.03/31.27 |

**说明**: 所有高效注意力方法都有一定精度损失，DSA 是唯一无损方案。

### Table 6: DSA RULER Results

| Model | 4K | 8K | 16K | 32K | 64K | 128K |
|-------|-----|-----|------|------|------|-------|
| GLM-4.7-Flash | 97.44 | 96.72 | 95.83 | 92.96 | 85.34 | 79.21 |
| + DSA warmup | 97.51 | 96.54 | 95.40 | 90.09 | 84.05 | 71.35 |
| + DSA full | 96.77 | 96.25 | 96.69 | 93.45 | 87.06 | 78.86 |

**说明**: DSA joint training 后几乎完全恢复 baseline 性能。

### Table 7: ARC Benchmark Comparison

| Benchmark | GLM-5 | GLM-4.7 | DeepSeek-V3.2 | Kimi-K2.5 | Claude Opus 4.5 | Gemini 3 Pro | GPT-5.2 |
|-----------|-------|---------|---------------|-----------|-----------------|--------------|---------|
| HLE | 30.5 | 24.8 | 25.1 | 31.5 | 28.4 | 37.2 | 35.4 |
| HLE (w/ Tools) | 50.4 | 42.8 | 40.8 | 51.8 | 43.4 | 45.8 | 45.5 |
| AIME 2026 I | 92.7 | 92.9 | 92.7 | 92.5 | 93.3 | 90.6 | - |
| SWE-bench Verified | 77.8 | 73.8 | 73.1 | 76.8 | 80.9 | 76.2 | 80.0 |
| SWE-bench Multilingual | 73.3 | 66.7 | 70.2 | 73.0 | 77.5 | 65.0 | 72.0 |
| Terminal-Bench 2.0 | 56.2/60.7† | 41.0/32.8 | 39.3/46.4 | 50.8 | 59.3/57.9 | 54.2 | 54.0 |
| BrowseComp | 62.0/75.9* | 52.0/67.5* | 51.4/67.6* | 60.6/74.9* | 37.0/57.8* | 37.8/59.2* | -/65.8* |
| τ²-Bench | 89.7 | 87.4 | 85.3 | 80.2 | 91.6 | 90.7 | 85.5 |
| Vending-Bench 2 | $4,432 | $2,377 | $1,034 | $1,198 | $4,967 | $5,478 | $3,591 |

**说明**: GLM-5 在开源模型中 SOTA，与 Claude Opus 4.5 和 GPT-5.2 相当。

### Table 8: CC-Bench-V2 Results

| Category | Task | Metric | GLM-5 | GLM-4.7 | Claude Opus 4.5 |
|----------|------|--------|-------|---------|-----------------|
| Frontend | HTML | ISR | 38.9 | 35.4 | 52.2 |
| Frontend | React | ISR | 34.6 | 17.2 | 39.7 |
| Frontend | Vue | ISR | 32.7 | 24.5 | 46.9 |
| Frontend | Build | BSR | 98.0% | 65-70% | 90-100% |
| Backend | Engineering | Pass@1 | 25.8 | 19.6 | 26.9 |
| Long-horizon | Repo Exploration | Pass@1 | 65.6 | 47.8 | 64.5 |
| Long-horizon | Chained Tasks | Pass@1 | 52.3 | 43.0 | 61.6 |

**说明**: GLM-5 在 backend 和 repo exploration 上与 Claude Opus 4.5 相当，但 frontend 和 chained tasks 有差距。

### Table 9: SWE-rebench Results

| Model | Resolved Rate | Pass@5 |
|-------|---------------|--------|
| Claude Opus 4.6 | 52.9% | 70.8% |
| GPT-5.2 (xhigh) | 51.7% | 58.3% |
| GLM-5 | 42.1% | 50.0% |
| GLM-4.7 | 41.3% | 56.3% |

**说明**: SWE-rebench 是动态 benchmark，GLM-5 可有效泛化到新 SWE 问题。

### Table 10: Model Architecture

| Component | GLM-4.5 | GLM-5 |
|-----------|---------|-------|
| Total Parameters | 355B | 744B |
| Activated Parameters | 32B | 40B |
| Dense Layers | 3 | 3 |
| MoE Layers | 89 | 75 |
| MTP Layers | 1 | 1 |
| Hidden Dim | 5120 | 6144 |
| # Experts | 160 | 256 |
| # Attention Heads | 96 | 64 |
| Vocabulary Size | 151552 | 154880 |

**说明**: GLM-5 规模翻倍，层数减少以降低 expert parallelism 通信开销。

### Table 11: Base Model Comparison

| Benchmark | DeepSeek-V3 Base | Kimi-K2 Base | GLM-4.5 Base | GLM-5 Base |
|-----------|------------------|--------------|--------------|------------|
| SimpleQA (EM) | 26.6 | 35.3 | 30.0 | 36.0 |
| MMLU (EM) | 87.2 | 87.8 | 86.1 | 88.3 |
| EvalPlus (Pass@1) | 65.6 | 80.3 | 78.1 | 87.0 |
| LiveCodeBench-Base | 24.6 | 26.3 | 28.1 | 34.4 |

**说明**: GLM-5-Base 在代码和知识 benchmarks 上表现优异。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Pre-training Corpus | 28.5T tokens | 代码/推理优先 + 长上下文 agent data | Base Model |
| SFT Data | - | 202K context, Interleaved/Preserved Thinking | Post-training |
| Reasoning RL | - | 数学/科学/代码/TIR 四域混合 | Post-training |
| Agentic RL | 10K+ | SWE/Terminal/Search 环境 | Post-training |
| CC-Bench-V2 | 220 tasks | Frontend/Backend/Long-horizon | Internal Eval |

### 实现细节

- **优化器**: Muon (矩阵正交化)
- **学习率**: 2e-4 → 4e-5 (pre-training), 4e-5 → 1e-5 (mid-training)
- **DSA Warmup**: 5e-3 → 2e-4 (1000 steps)
- **Batch Size**: GRPO group size 32, batch size 32
- **RL 超参**: $\beta=2, \epsilon_{low}=0.2, \epsilon_{high}=0.28$
- **硬件**: 国产芯片平台（华为 Ascend 等 7 个）

---

## 批判性思考

### 优点

1. **架构创新**: DSA 在不损失性能的情况下实现长上下文计算效率提升
2. **训练系统**: 异步 RL 框架解决长 horizon agent 任务训练瓶颈
3. **工程导向**: CC-Bench-V2 等真实场景 benchmark，超越静态 leaderboard
4. **开源贡献**: 首个 Intelligence Index v4.0 达 50 分的开源权重模型

### 局限性

1. **Frontend Gap**: ISR 与 Claude Opus 4.5 仍有差距（52.2 vs 38.9）
2. **Chained Tasks**: 长链任务错误累积问题，Pass@1 低于 Claude（61.6 vs 52.3）
3. **Math 性能**: GSM8K/MATH 基础模型分数下降（68.8/56.4），可能因代码优先训练
4. **依赖 DeepSeek 技术**: DSA、MLA 等核心技术来自 DeepSeek 系列

### 潜在改进方向

1. **增强 Long-horizon Self-correction**: 减少 chained tasks 中的错误累积
2. **平衡 Code/Math Training**: 提升数学推理能力
3. **Frontend Engineering**: 加强 GUI 理解和交互能力
4. **原创架构创新**: 探索自主设计的稀疏注意力机制

### 可复现性评估

- [x] 代码开源 (GitHub: zai-org/GLM-5)
- [ ] 预训练模型（声称开源，待发布）
- [x] 训练细节完整（40 页技术报告）
- [x] 评估 benchmark 公开

---

## 关联笔记

### 基于

- [[DeepSeek-V3]]: DSA、MLA、MTP 架构来源
- [[GRPO]]: RL backbone 基础
- [[IcePop]]: training-inference mismatch 处理技术

### 对比

- [[Claude Opus 4.5]]: 真实工程任务的主要对比基准
- [[GPT-5.2]]: 经济任务 benchmark 对比
- [[Gemini 3 Pro]]: 多模态能力对比

### 方法相关

- [[Mixture of Experts]]: 核心架构
- [[DeepSeek Sparse Attention]]: 长上下文效率技术
- [[Multi-latent Attention]]: KV cache 优化
- [[Multi-token Prediction]]: speculative decoding
- [[On-policy Cross-Stage Distillation]]: 能力恢复技术
- [[Asynchronous Agent RL]]: agent 训练框架
- [[TITO]]: 异步 RL 的 token 级一致性
- [[Muon Optimizer]]: 矩阵正交化优化器

### 硬件/数据相关

- [[华为 Ascend]]: 国产芯片适配案例
- [[SWE-bench]]: 软件工程 benchmark
- [[Vending-Bench]]: 长 horizon 商业任务 benchmark

---

## 速查卡片

> [!summary] GLM-5
> - **核心**: 744B MoE + DSA 架构，异步 Agent RL 训练系统
> - **方法**: MLA + DSA + MTP + 多阶段 RL Pipeline
> - **结果**: Intelligence Index 50（开源 SOTA），真实工程任务接近 Claude Opus 4.5
> - **代码**: https://github.com/zai-org/GLM-5

---

*笔记创建时间: 2026-04-17*