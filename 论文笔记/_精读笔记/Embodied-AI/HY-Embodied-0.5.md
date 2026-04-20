---
title: "HY-Embodied-0.5: Embodied Foundation Models for Real-World Agents"
method_name: "HY-Embodied-0.5"
authors: [Tencent Robotics X, HY Vision Team, Xumin Yu, Zuyan Liu, Ziyi Wang, He Zhang, Yongming Rao, Fangfu Liu, Yani Zhang, Ruowen Zhao, Oran Wang, Yves Liang, Haitao Lin, Minghui Wang, Yubo Dong, Kevin Cheng, Bolin Ni, Rui Huang, Han Hu, Zhengyou Zhang, Linus, Shunyu Yao]
year: 2026
venue: arXiv
tags: [embodied-ai, vision-language-action, mixture-of-transformers, robot-control, spatial-reasoning, vla]
zotero_collection: Embodied-AI
image_source: online
arxiv_html: https://arxiv.org/html/2604.07430v1
created: 2026-04-20
---

# 论文笔记：HY-Embodied-0.5: Embodied Foundation Models for Real-World Agents

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Tencent Robotics X, HY Vision Team |
| 日期 | April 2026 |
| 项目主页 | https://github.com/Tencent-Hunyuan/HY-Embodied |
| 对比基线 | [[Qwen3-VL]], [[RoboBrain]], [[Pi05]], [[Gemini-3.0-Pro]] |
| 链接 | [arXiv](https://arxiv.org/abs/2604.07430) / [Code](https://github.com/Tencent-Hunyuan/HY-Embodied) |

---

## 一句话总结

> 腾讯推出专为实体智能体设计的视觉-语言基础模型，采用 MoT 架构和迭代式后训练策略，在22个空间/具身理解基准上超越同类模型，并在真实机器人控制任务中取得优秀表现。

---

## 核心贡献

1. **Mixture-of-Transformers (MoT) 架构**: 为视觉和语言模态设计独立的计算分支，通过模态自适应计算增强视觉感知能力，同时保持语言能力不退化。
2. **视觉潜在Token (Visual Latent Tokens)**: 在视觉输入序列末尾添加可学习的潜在Token，连接视觉和语言模态，提升小模型的感知表达能力。
3. **迭代式自进化后训练范式**: 结合强化学习 (RL) 和拒绝采样微调 (RFT) 的迭代训练，以及大模型到小模型的在线策略蒸馏，有效迁移推理能力。

---

## 问题背景

### 要解决的问题
通用视觉-语言模型 (VLMs) 在物理世界实体任务中存在两大核心缺陷：
1. **细粒度视觉感知不足**: 无法精确捕捉物理 grounding 所需的细节信息
2. **具身预测/交互/规划能力缺失**: 主流 VLMs 在静态数据集上训练，缺乏面向动作的能力

### 现有方法的局限
- 通用 VLMs ([[Qwen3-VL]], [[Gemini]]) 侧重语义理解，空间感知能力弱
- 专业具身 VLMs ([[RoboBrain]]) 参数规模受限，推理深度不足
- 传统 VLA 模型 ([[Pi0]]) 缺乏强大的视觉-语言基础支撑

### 本文的动机
实体 VLMs 是连接数字智能与物理智能的关键桥梁，通过增强空间/时序感知和具身推理能力，可将大规模多模态知识应用于真实世界任务。

---

## 方法详解

### 模型架构

HY-Embodied-0.5 采用 **VLM + MoT** 架构：
- **输入**: 语言指令 $l$ + 视觉观测 $o_t$ + 状态 $s_t$
- **视觉编码器**: [[HY-ViT-2.0]] (400M 参数，支持任意分辨率输入)
- **核心模块**: [[Mixture-of-Transformers]] 用于模态自适应计算
- **视觉潜在Token**: 连接视觉和语言模态
- **输出**: 具身理解/规划结果，或 VLA 模型的 [[Action Chunking|动作块]] $a_{t:t+k}$
- **总参数**: MoT-2B (2B激活/4B总量) + MoE-A32B (32B激活/407B总量)

### 核心模块

#### 模块1: HY-ViT 2.0 - 高效原生分辨率视觉编码器

**设计动机**: 实体任务需要处理任意分辨率的视觉输入，传统固定分辨率 ViT 限制了感知精度。

**具体实现**:
- 支持任意分辨率输入，采用 NaViT 风格的 patch packing
- 通过大型内部 ViT 的知识蒸馏获得高效表示
- 引入小型 LLM 提供语言监督信号
- 使用视觉重建监督确保信息无损
- 离散表示: codebook size 2k，每 8×8 patch 压缩为单个离散码

#### 模块2: Mixture-of-Transformers (MoT) - 模态自适应计算

**设计动机**: 大量视觉训练会损害语言能力，MoT 通过独立参数解耦视觉和语言计算。

**具体实现**:
- 复制 FFN 和 QKV 参数用于视觉分支，文本分支使用原始参数
- 视觉 Token 使用双向注意力（视觉数据无单向特性）
- 文本 Token 保持因果注意力
- 引入视觉下一码预测任务监督视觉分支
- 对推理效率影响极小（解码阶段占主导）

#### 模块3: Visual Latent Tokens - 视觉-语言连接器

**设计动机**: 受 latent thinking 和 vision registers 启发，小 VLM 需要额外机制连接视觉和语言。

**具体实现**:
- 在每个视觉元素（图像/视频帧）末尾添加可学习潜在 Token
- 预训练阶段使用大型 ViT 的全局特征监督其输出
- 有效聚合视觉信息并与语言概念对齐

---

## 关键公式

### 公式1: [[Vision Loss|视觉下一码预测损失]]

$$
\mathcal{L}_{\text{vision}}=-\frac{1}{N_{v}}\sum_{i=1}^{N_{v}}\log p_{i}(z_{i})
$$

**含义**: 使用大型 ViT 生成的离散码监督视觉分支的下一 patch 预测。

**符号说明**:
- $N_{v}$: 视觉 Token 数量
- $p_{i}$: 第 $i$ 个 Token 的预测概率分布
- $z_{i}$: 教师 ViT 生成的目标离散码

### 公式2: [[Global Loss|全局特征对齐损失]]

$$
\mathcal{L}_{\text{global}}=-\frac{f_{\text{latent}}^{\top}f_{\text{teacher}}}{\|f_{\text{latent}}\|\|f_{\text{teacher}}\|}
$$

**含义**: 将视觉潜在 Token 的特征与教师 ViT 的全局 CLS 特征对齐。

**符号说明**:
- $f_{\text{latent}}$: 视觉潜在 Token 的映射隐藏状态
- $f_{\text{teacher}}$: 教师 ViT 提取的全局 CLS 特征

### 公式3: 预训练总损失

$$
\mathcal{L}_{\text{total}}=\mathcal{L}_{\text{llm}}+\mathcal{L}_{\text{vision}}+\mathcal{L}_{\text{global}}
$$

**含义**: 预训练阶段联合优化语言损失、视觉损失和全局损失。中训练和微调阶段仅使用 $\mathcal{L}_{\text{llm}}$。

### 公式4: [[Task-Aware Reward|任务感知奖励函数]]

$$
r=R_{t}(y,y^{\star})\in[0,1].
$$

**含义**: 根据输出结构设计不同奖励函数，几何任务使用密集奖励，离散任务使用精确匹配，开放推理使用 LLM 评判。

**符号说明**:
- $R_{t}$: 任务类型对应的奖励函数
- $y$: 模型输出
- $y^{\star}$: 参考答案

### 公式5: [[GRPO|GRPO 优势函数]]

$$
A_{i}=\frac{r_{i}-\mu(\mathbf{r})}{\sigma(\mathbf{r})},\qquad\mathbf{r}=\{r_{1},\dots,r_{G}\},
$$

**含义**: 在采样组内标准化奖励，计算相对优势，适配具身任务的异质性。

**符号说明**:
- $A_{i}$: 第 $i$ 个响应的优势值
- $\mu(\mathbf{r})$, $\sigma(\mathbf{r})$: 组内奖励的均值和标准差
- $G$: 采样组大小（默认16）

### 公式6: [[PPO Loss|GRPO 策略损失]]

$$
\mathcal{L}_{\mathrm{RL}}(x)=-\frac{1}{\sum_{i=1}^{G}|y_{i}|}\sum_{i=1}^{G}\sum_{t=1}^{|y_{i}|}\min\!\Big(\rho_{i,t}A_{i},\,\mathrm{clip}(\rho_{i,t},1-\epsilon_{\mathrm{low}},1+\epsilon_{\mathrm{high}})A_{i}\Big),
$$

**含义**: 使用不对称裁剪（[0.8, 1.35]）的 PPO 目标优化策略。

**符号说明**:
- $\rho_{i,t}$: 新旧策略概率比 $\frac{\pi_{\theta}(y_{i,t})}{\pi_{\theta_{\text{old}}}(y_{i,t})}$
- $\epsilon_{\text{low}}=0.2$, $\epsilon_{\text{high}}=0.35$: 裁剪边界

### 公式7: [[On-Policy Distillation|在线策略蒸馏损失]]

$$
\mathcal{L}_{\mathrm{OPD}}=\mathbb{E}_{x,\,y\sim\pi_{s}(\cdot\mid x)}\left[\frac{1}{|y|}\sum_{t=1}^{|y|}\mathrm{KL}\!\left(\pi_{t}(\cdot\mid x,y_{<t})\,\|\,\pi_{s}(\cdot\mid x,y_{<t})\right)\right].
$$

**含义**: 学生模型先采样自身响应，教师在相同前缀上提供监督，减少训练-推理分布差距。

**符号说明**:
- $\pi_{t}$, $\pi_{s}$: 教师/学生模型的下一 Token 分布
- $y_{<t}$: 已生成的前缀 Token

---

## 关键图表

### Figure 1: Overview / 系统概览

![Figure 1](https://arxiv.org/html/2604.07430v1/2604.07430v1/x1.png)

**说明**: HY-Embodied-0.5 MoT-2B 在空间和具身基准以及下游机器人控制任务上的表现。模型推进了具身 VLM 的前沿，并在真实世界机器人评估中表现出色。

### Figure 2: MoT Architecture / 模型架构

![Figure 2](https://arxiv.org/html/2604.07430v1/2604.07430v1/x2.png)

**说明**: [[Mixture-of-Transformers]] 架构。MoT 设计通过模态特定的 QKV 和 FFN 层解耦视觉和文本 Token 处理。视觉潜在 Token 和混合优化损失在大规模训练中连接并强化模态间关系。

### Figure 3: MoT Attention Computation / 注意力计算

![Figure 3](https://arxiv.org/html/2604.07430v1/2604.07430v1/x3.png)

**说明**: 模态自适应 MoT 的注意力计算可视化。在交错多模态序列中用不同颜色展示视觉和文本注意力的独立计算。

### Figure 4: Pre-training Data Distribution / 数据分布

![Figure 4](https://arxiv.org/html/2604.07430v1/2604.07430v1/x4.png)

**说明**: 预训练和中训练阶段的数据分布。预训练涵盖超过 200B tokens（空间、机器人、视觉感知任务），中训练使用超过 12M 高质量 QA 数据。

### Figure 5: Training Pipeline / 训练流程

![Figure 5](https://arxiv.org/html/2604.07430v1/2604.07430v1/x5.png)

**说明**: HY-Embodied-0.5 系列的训练流程。大规模预训练建立基础多模态表示，具身后训练通过迭代自进化增强复杂推理，最后通过在线策略蒸馏将知识迁移到边缘部署模型。

### Figure 6: Reward Designs / RL 奖励设计

![Figure 6](https://arxiv.org/html/2604.07430v1/2604.07430v1/x6.png)

**说明**: 具身强化学习的奖励设计。分为四类：Grounding-Based（空间定位）、Regression-Based（数值估计）、Trajectory-Based（运动规划）、Textual-Based（语义推理）。

### Figure 7: General Benchmarks / 通用基准对比

![Figure 7](https://arxiv.org/html/2604.07430v1/2604.07430v1/x7.png)

**说明**: 与规模匹配的通用 VLMs 在通用理解基准上的对比。HY-Embodied-0.5 MoT-2B 在保持具身和空间理解优势的同时，在通用视觉任务上也保持竞争力。

### Figure 8: Visual Perception Visualization / 视觉感知可视化

![Figure 8](https://arxiv.org/html/2604.07430v1/2604.07430v1/x8.png)

**说明**: 视觉感知任务的可视化结果。HY-Embodied-0.5 MoT-2B 在深度估计、目标检测和复杂计数任务上表现优于其他具身和通用 VLMs。

### Figure 9: Embodied Tasks Visualization / 具身任务可视化

![Figure 9](https://arxiv.org/html/2604.07430v1/2604.07430v1/x9.png)

**说明**: 具身任务的可视化结果。模型展示精确的视觉定位、逻辑动作规划和场景理解能力。

### Figure 10: Chain-of-Thought Illustration / CoT 思维链

![Figure 10](https://arxiv.org/html/2604.07430v1/2604.07430v1/x10.png)

**说明**: CoT 思维链过程展示。模型在解决复杂视觉和具身问题时进行逐步空间关系分析和可供性评估，展现出高级的自我反思和错误修正机制。

### Figure 11: MoT Efficiency / MoT 效率分析

![Figure 11](https://arxiv.org/html/2604.07430v1/2604.07430v1/x11.png)

**说明**: MoT 架构效率分析。训练收敛更快（左图），推理速度与 Dense-2B 相当（右图），总推理时间、FLOPs、prefill/decode 时间详细对比。

### Figure 12: Visual Latent Token Attention / 潜在 Token 注意力

![Figure 12](https://arxiv.org/html/2604.07430v1/2604.07430v1/x12.png)

**说明**: 视觉潜在 Token 的注意力可视化。视觉注意力精确定位显著目标和关键空间区域，语言注意力聚焦核心语义实体和动作指令。

### Figure 13: Robot Control Results / 机器人控制实验

![Figure 13](https://arxiv.org/html/2604.07430v1/2604.07430v1/x13.png)

**说明**: 真实机器人实验结果。三个任务：Precision Plug-in Packing (85%)、Tableware Stacking (80%)、Mug Hanging (75%)。HY-Embodied-0.5 VLA 超越 $\pi_0$ 和 $\pi_0.5$ 基线。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Omni-Detection | 62M | 2D/3D检测，包含 OpenImages, Objects365, RefCOCO, SA-1B | 预训练 |
| Depth Estimation | 36M | 绝对/相对深度，室内/室外/自动驾驶 | 预训练 |
| Segmentation | 5M | SA-1B 高分辨率分割图 | 预训练 |
| Pointing & Counting | 11M | Pixmo-Points 等点级标注 | 预训练 |
| Embodied Data | 多源 | Grounding/Affordance/Trajectory/Planning/Reasoning | 预训练+中训练 |
| Spatial Data | 多源 | ScanNet/ScanNet++/ARKitScenes | Correspondence/Geometry/Configuration/Measurement/Dynamics |
| UMI Data | 5K hours | 通用机器人操作数据 | VLA 微调 |

### 实现细节

**预训练阶段**:
- **Backbone**: HY-ViT 2.0 (400M) + Hunyuan-1.8B LLM
- **学习率**: 5e-5 (LLM), 5e-6 (ViT)
- **Weight decay**: 1e-4
- **Batch size**: 256
- **Context length**: 32k tokens
- **训练数据**: 600B+ tokens (389B通用 + 236B具身/感知)

**中训练阶段**:
- **数据**: 30M 实例 (通用:具身:空间 = 12:5:3)
- **学习率**: 5e-5 + cosine decay
- **ViT**: frozen

**RL 阶段**:
- **Batch size**: 128
- **学习率**: 8×10⁻⁷
- **组大小**: 16
- **Epochs**: 5
- **裁剪范围**: [0.8, 1.35]

**VLA 微调**:
- **UMI 数据**: 5K hours, batch size 32×32 GPUs, 200K iterations
- **真实机器人数据**: 300-700 episodes per task

### 评估基准 (22个)

**视觉感知**:
- CV-Bench, DA-2K

**具身理解**:
- ERQA, EmbSpatial-Bench, RoboBench-MCQ, RoboBench-Planning, RoboSpatial-Home, ShareRobot-Affordance, ShareRobot-Trajectory, Ego-Plan2

**空间理解**:
- 3DSRBench, All-Angles Bench, MindCube, MMSI-Bench, RefSpatial-Bench, SAT, SIBench-mini, SITE-Bench-Image, SITE-Bench-Video, ViewSpatial, VSIBench, Where2Place

---

## 实验结果

### Table 1: MoT-2B 在22个基准上的表现

| Capability | Benchmark | HY-Embodied-0.5 MoT-2B | Qwen3-VL 2B | Qwen3-VL 4B | RoboBrain 2.5 4B | MiMo-Embodied 7B |
|------------|-----------|------------------------|-------------|-------------|-------------------|------------------|
| Visual Perception | CV-Bench | **89.2** | 80.0 | 85.7 | 86.9 | 88.8 |
| Visual Perception | DA-2K | **92.3** | 69.5 | 76.5 | 79.4 | 72.2 |
| Embodied | ERQA | **54.5** | 41.8 | 47.3 | 43.3 | 46.8 |
| Embodied | EmbSpatial-Bench | **82.8** | 75.9 | 80.7 | 73.8 | 76.2 |
| Embodied | RoboBench-MCQ | **49.2** | 36.9 | 45.8 | 44.4 | 43.6 |
| Embodied | RoboBench-Planning | 54.2 | 36.2 | 36.4 | 39.2 | **58.7** |
| Embodied | RoboSpatial-Home | 55.7 | 45.3 | **63.2** | 62.3 | 61.8 |
| Embodied | ShareRobot-Aff. | **26.8** | 19.8 | 25.5 | 25.5 | 9.0 |
| Embodied | ShareRobot-Traj. | 73.3 | 41.6 | 62.2 | **81.4** | 50.6 |
| Embodied | Ego-Plan2 | 45.5 | 35.5 | 38.8 | **52.6** | 39.9 |
| Spatial | 3DSRBench | **57.0** | 39.9 | 43.9 | 44.8 | 42.0 |
| Spatial | All-Angles Bench | **55.1** | 42.3 | 46.7 | 43.8 | 49.0 |
| Spatial | MindCube | **66.3** | 28.4 | 31.0 | 26.9 | 36.2 |
| Spatial | MMSI-Bench | **33.2** | 23.6 | 25.1 | 20.5 | 31.9 |
| Spatial | RefSpatial-Bench | 45.8 | 28.9 | 45.3 | **56.0** | 48.0 |
| Spatial | SAT | 76.7 | 45.3 | 56.7 | 51.3 | **78.7** |
| Spatial | SIBench-mini | **58.2** | 42.0 | 50.9 | 47.3 | 53.1 |
| Spatial | SITE-Bench-Image | **62.7** | 52.3 | 61.0 | 57.9 | 49.9 |
| Spatial | SITE-Bench-Video | **63.5** | 52.2 | 58.0 | 54.8 | 58.9 |
| Spatial | ViewSpatial | **53.1** | 37.2 | 41.6 | 36.6 | 36.1 |
| Spatial | VSIBench | **60.5** | 48.0 | 55.2 | 41.7 | 48.5 |
| Spatial | Where2Place | **68.0** | 45.0 | 59.0 | 65.0 | 63.6 |

**关键发现**: MoT-2B 在 16/22 基准上取得最佳成绩，平均得分 58.0%，超越参数更大的 Qwen3-VL-4B (10.2%) 和 RoboBrain2.5-4B (8.6%)。

### Table 2: MoE-A32B 与前沿模型对比

| Capability | Benchmark | HY-Embodied A32B | Kimi K2.5 | Seed 2.0 | Qwen 3.5 A17B | Gemini 3.0 Pro |
|------------|-----------|------------------|-----------|----------|---------------|----------------|
| Visual Perception | CV-Bench | 88.8 | **89.0** | 88.5* | 88.6 | 85.4* |
| Visual Perception | DA-2K | 90.2 | 83.4 | **92.3*** | 83.3 | 83.6* |
| Embodied | ERQA | 62.3 | 59.8 | 61.8* | 61.0 | **65.0*** |
| Embodied | EmbSpatial-Bench | **84.1** | 81.5 | 81.0* | 83.8 | 83.6* |
| Embodied | RoboBench-MCQ | 62.8 | 59.0 | 66.5* | 63.8 | **69.2*** |
| Embodied | RoboBench-Planning | 59.3 | **60.0** | 60.1* | 56.7 | 60.0* |
| Embodied | RoboSpatial-Home | **76.6** | 66.0 | 71.7* | 74.9 | 57.1* |
| Embodied | ShareRobot-Aff. | 28.6 | 21.5 | 27.5* | **29.3** | 24.8* |
| Embodied | ShareRobot-Traj. | **76.9** | 68.5 | 71.8* | 73.8 | 68.7* |
| Embodied | Ego-Plan2 | 51.4 | 47.4 | 56.6* | 55.3 | **60.0*** |
| Spatial | 3DSRBench | 56.6 | 55.9 | 58.2* | 56.6 | **58.3*** |
| Spatial | All-Angles Bench | 71.8 | 64.8 | 69.3* | 72.1 | **73.4*** |
| Spatial | MindCube | **69.2** | 57.8 | 55.2* | 59.0 | 66.0* |
| Spatial | MMSI-Bench | 39.2 | 36.5 | 47.6* | 43.8 | **48.0*** |
| Spatial | RefSpatial-Bench | 57.2 | 43.3 | **72.2*** | 61.0 | 33.2* |
| Spatial | SAT | 87.3 | 79.3 | 86.2* | 86.0 | **88.0*** |
| Spatial | SIBench-mini | 67.3 | 63.0 | 65.9* | 66.3 | **68.0*** |
| Spatial | SITE-Bench-Image | 74.7 | 73.8 | 75.6* | **77.1** | 75.4* |
| Spatial | SITE-Bench-Video | **72.5** | 71.5 | 68.9* | 72.3 | 69.8* |
| Spatial | ViewSpatial | **59.8** | 45.2 | 56.4* | 52.2 | 50.8* |
| Spatial | VSIBench | **68.3** | 54.2 | 51.0* | 61.1 | 57.9* |
| Spatial | Where2Place | 70.0 | 64.0 | 73.0* | **76.0** | 52.0* |

**平均得分**: HY-Embodied A32B **67.0%**, 超越 Gemini 3.0 Pro (63.6%), Seed 2.0 (66.2%), Qwen 3.5 A17B (66.1%), Kimi K2.5 (61.1%).

---

## 批判性思考

### 优点
1. **架构创新**: MoT 有效解耦视觉和语言计算，解决了"视觉训练损害语言能力"的痛点
2. **数据规模**: 100M+ 具身感知数据，覆盖从底层感知到高层规划的完整层级
3. **训练策略完整**: 预训练→中训练→SFT→RL→RFT→蒸馏的全流程设计清晰
4. **开源贡献**: 代码和模型完全开源，有利于社区发展
5. **真实验证**: 在真实机器人任务上取得可量化成果，而非仅停留在仿真层面

### 局限性
1. **MoE 模型未开源**: 仅开源 MoT-2B 小模型，407B 参数的 MoE-A32B 未公开
2. **依赖 UMI 数据**: VLA 部分依赖大量通用机器人数据，数据获取门槛高
3. **任务覆盖有限**: 机器人实验仅涉及三个特定任务，泛化能力待验证
4. **推理时延**: thinking mode 需要更多计算，边缘部署的实际响应速度待评估

### 潜在改进方向
1. **扩展动作模态**: MoT 架构可进一步扩展到动作 Token，实现真正的三模态自适应
2. **降低数据依赖**: 探索更高效的具身数据合成或自监督学习方案
3. **多具身泛化**: 当前主要针对双臂机器人，可扩展到移动机器人、无人机等
4. **实时推理优化**: 研究 thinking mode 的自适应长度或早停策略

### 可复现性评估
- [x] 代码开源 (GitHub)
- [x] 预训练模型 (MoT-2B)
- [ ] 大模型权重 (MoE-A32B 未公开)
- [x] 训练细节完整 (论文详述各阶段超参数)
- [ ] 数据集可获取 (内部数据未公开，仅使用公开数据源)

---

## 关联笔记

### 基于
- [[Mixture-of-Transformers]]: 核心架构设计
- [[NaViT]]: 原生分辨率 ViT 设计
- [[Vision-Registers]]: 视觉潜在 Token 设计灵感
- [[DeepSeek-R1]]: GRPO 强化学习框架
- [[Pi0]] / [[Pi05]]: VLA 模型结构参考

### 对比
- [[Qwen3-VL]]: 通用 VLM 基线
- [[RoboBrain]]: 专业具身 VLM 基线
- [[Gemini-3.0-Pro]]: 前沿 VLM 对比

### 方法相关
- [[Mixture-of-Transformers]]: 核心架构
- [[Vision-Language-Action]]: 下游应用范式
- [[Chain-of-Thought]]: 思维链推理
- [[GRPO]]: 强化学习优化
- [[On-Policy-Distillation]]: 知识蒸馏方法

### 硬件/数据相关
- [[UMI-Dataset]]: 通用机器人操作数据
- [[ScanNet]] / [[ScanNet++]]: 空间理解数据源
- [[DROID]]: 大规模机器人数据集

---

## 速查卡片

> [!summary] HY-Embodied-0.5: Embodied Foundation Models
> - **核心**: 腾讯具身 VLM，MoT 架构 + 迭代式后训练 + 在线蒸馏
> - **方法**: HY-ViT 2.0 视觉编码 + MoT 模态自适应 + Visual Latent Tokens + GRPO + RFT
> - **结果**: MoT-2B 16/22 基准最佳 (平均58.0%)，A32B 平均67.0% 超越 Gemini 3.0 Pro
> - **代码**: [GitHub](https://github.com/Tencent-Hunyuan/HY-Embodied)

---

*笔记创建时间: 2026-04-20*