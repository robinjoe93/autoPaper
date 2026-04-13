---
title: "Exploring Conditions for Diffusion Models in Robotic Control"
method_name: "ORCA"
authors: [Heeseong Shin, Byeongho Heo, Dongyoon Han, Seungryong Kim, Taekyung Kim]
year: 2026
venue: CVPR 2026
tags: [imitation-learning, diffusion-model, robotic-control, visual-representation, prompt-learning]
zotero_collection: _待整理
image_source: online
arxiv_html: https://arxiv.org/html/2510.15510v2
created: 2026-04-13
---

# 论文笔记：Exploring Conditions for Diffusion Models in Robotic Control

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | KAIST AI, NAVER AI Lab |
| 日期 | October 2025 (revised April 2026) |
| 项目主页 | https://orca-rc.github.io/ |
| 对比基线 | [[SCR]], [[VC-1]], [[TADP]] |
| 链接 | [arXiv](https://arxiv.org/abs/2510.15510) / [Code](https://github.com/) (Coming Soon) |

---

## 一句话总结

> ORCA 提出可学习的 task prompts 和 visual prompts 来条件化扩散模型，实现机器人控制任务的 task-adaptive 视觉表征，在多个 benchmark 上取得 SOTA。

---

## 核心贡献

1. **发现文本条件失效**: 证明在机器人控制任务中，文本条件（包括 VLM 生成的 captions）效果不佳甚至有害，归因于 domain gap
2. **提出 ORCA 框架**: 设计 learnable task prompts（任务自适应）+ visual prompts（帧级细粒度细节）替代文本条件
3. **全面的实验验证**: 在 DeepMind Control、MetaWorld、Adroit 共 12 个任务上超越所有 baseline

---

## 问题背景

### 要解决的问题

预训练视觉表征（如 CLIP、VC-1）在模仿学习中是 **task-agnostic** 的 —— 在策略学习中保持 frozen，无法适应不同任务的具体需求。这导致：
- 不同 representation 在不同任务上表现差异很大
- 需要手动逐任务选择最佳 representation
- Fine-tuning 会因过拟合导致性能崩溃

### 现有方法的局限

1. **Task-agnostic 方法**（CLIP, VC-1, SCR）: 无法适应任务特定需求
2. **Fine-tuning**: 在有限 imitation learning data 上严重过拟合，性能下降超过 80%
3. **文本条件方法**（在其他 vision tasks 成功）: 在控制环境中 cross-attention grounding 不准确

### 本文的动机

Text-to-image diffusion models（如 Stable Diffusion）在其他 vision tasks（语义分割、深度估计）中通过文本条件显著提升性能。作者探索将这一机制迁移到机器人控制，但发现：

- 文本条件在控制任务中失效 → 原因是 **domain gap**（模型训练于真实世界图像，控制环境是模拟机器人）
- 控制任务需要 **frame-level** 条件（动态视频流）+ **fine-grained** 视觉细节
- 提出：用 **visual information** 替代/补充文本，通过 **learnable prompts** 实现任务自适应

---

## 方法详解

### 模型架构

ORCA 采用 **条件化扩散模型** 架构：

- **输入**: 观测图像 $I_o^i$
- **Backbone**: Stable Diffusion v1.5 (SD 1.5) U-Net
- **核心模块**: [[Task Prompt]] + [[Visual Prompt]] 作为条件 $\mathcal{C}^*$
- **视觉编码器**: [[DINOv2]] 用于提取 dense visual features
- **输出**: 视觉表征 → Policy Network → 动作 $a_o^i$
- **训练目标**: [[Behavior Cloning]] 损失

### 核心模块

#### 模块1: Task Prompts

**设计动机**: 文本条件因 domain gap 导致 grounding 不准确，改用 learnable tokens 隐式学习任务相关的关注区域

**具体实现**:
- 实现为 learnable parameters $p_t$，长度 $l_t=4$
- 在单个任务中所有观测共享
- 通过 cross-attention 注入 U-Net
- 与 policy network 一起通过 BC 损失优化

**效果**: Cross-attention maps 自动聚焦于任务关键区域（如 Button-press 中的按钮和机械臂）

#### 模块2: Visual Prompts

**设计动机**: 控制任务需要每帧的 fine-grained 视觉细节（如机械臂不同部位的精确位置），文本无法提供这种 frame-level 信息

**具体实现**:
- 使用预训练 [[DINOv2]] $\mathcal{E}_V$ 提取 dense visual features
- 通过小卷积层压缩为 visual tokens $p_v$，长度 $l_v=16$
- 与 task prompts 一起输入 text encoder $\tau_\theta$
- 输出条件 $\mathcal{C}^* = \tau_\theta(p_t; p_v)$

**效果**: Attention maps 能区分细节区域（如 Cheetah 的前后腿）

---

## 关键公式

### 公式1: [[Diffusion Process|噪声添加]]

$$
z_t = \sqrt{\bar{\alpha}_t} z_0 + \sqrt{1-\bar{\alpha}_t} \epsilon
$$

**含义**: 将干净图像 $z_0$ 在 timestep $t$ 噪声化为 $z_t$

**符号说明**:
- $z_0 = \mathcal{E}(I)$: VQGAN encoder 编码的干净 latent
- $\bar{\alpha}_t = \prod_{i=1}^t \alpha_i$: 累积噪声 schedule
- $\epsilon \sim \mathcal{N}(0, I)$: Gaussian 噪声

### 公式2: [[Diffusion Training|扩散模型训练目标]]

$$
\mathcal{L}_{\text{DM}} = \mathbb{E}_{z_0, \epsilon, t} \left[ \|\epsilon - \epsilon_\theta(z_t(z_0, \epsilon), t; \mathcal{C})\|_2^2 \right]
$$

**含义**: 训练 denoising network $\epsilon_\theta$ 预测添加的噪声

**符号说明**:
- $\epsilon_\theta$: U-Net denoising network
- $\mathcal{C}$: 条件（文本 prompt 或 ORCA 的 task/visual prompts）

### 公式3: [[Behavior Cloning|策略学习目标]]

$$
\mathcal{L}_{\text{BC}(\phi, \mathbf{p})} = \sum_{i=1}^N \sum_o \| \pi_\phi(\epsilon_\theta(z_t, t; \mathcal{C}^*)) - a_o^i \|
$$

**含义**: 通过 Behavior Cloning 损失同时优化 policy network $\pi_\phi$、task prompts $p_t$ 和 visual prompts $p_v$

**符号说明**:
- $\pi_\phi$: Policy network 参数为 $\phi$
- $z_t = \sqrt{\bar{\alpha}_t} \mathcal{E}(I_o^i) + \sqrt{1-\bar{\alpha}_t} \epsilon$: 噪声化的观测 latent
- $\mathcal{C}^* = \tau_\theta(p_t; p_v)$: ORCA 条件，来自 text encoder
- $a_o^i$: 第 $i$ 条轨迹第 $o$ 步的真实动作

---

## 关键图表

### Figure 1: Overview / 系统概览

![Figure 1](https://orca-rc.github.io/images/teaser.png)

**说明**: ORCA 整体架构。提出 task prompts 和 visual prompts 替代文本条件，实现 task-adaptive 的扩散模型条件化。

### Figure 2: Motivation / 动机

![Figure 2](https://orca-rc.github.io/images/motivation.png)

**说明**: 对比 task-agnostic（冻结表征）vs task-adaptive（条件化）方法。探索文本条件在控制任务中的有效性。

### Figure 3: Case Study / 案例分析

![Figure 3](https://orca-rc.github.io/images/case.png)

**说明**: Cross-attention maps 可视化。
- (a) 文本条件在某些任务有益，某些有害
- (b) Button-press: 文本 grounding 较好
- (c) Cheetah-run: 文本 grounding 混乱，ORCA 的 task/visual prompts 能准确聚焦关键区域

### Figure 4: Proposed Framework / ORCA 架构

![Figure 4](https://orca-rc.github.io/images/method.png)

**说明**: Task prompts 在任务内共享，隐式描述任务；Visual prompts 从 DINOv2 提取 dense features，捕获帧级细节。

### Table 1: DeepMind Control + MetaWorld 结果

| Methods | Backbone | Stand | Walk | Reacher | Cheetah | Finger | Mean | Assembly | Bin-pick | Button | Drawer | Hammer | Mean |
|---------|----------|-------|------|---------|---------|--------|------|----------|----------|---------|--------|--------|------|
| CLIP | ViT-L/16 | 87.3 | 58.3 | 54.5 | 29.9 | 67.5 | 59.5 | 85.3 | 69.3 | 60.0 | 100.0 | 92.0 | 81.3 |
| VC-1 | ViT-L/16 | 86.1 | 54.3 | 18.3 | 40.9 | 65.7 | 53.1 | 93.3 | 61.3 | 73.3 | 100.0 | 93.3 | 84.2 |
| SCR | SD 1.5 | 85.5 | 64.3 | 81.8 | 43.4 | 66.6 | 68.3 | 92.0 | 86.7 | 74.7 | 100.0 | 98.7 | 90.4 |
| TADP | SD 1.5 | 89.0 | 69.9 | 86.6 | 41.1 | 66.9 | 70.7 | 96.0 | 90.7 | 80.0 | 100.0 | 96.0 | 93.1 |
| **ORCA** | SD 1.5 | **89.1** | **76.9** | **87.6** | **50.0** | **68.0** | **74.3** | **98.7** | **90.7** | **88.0** | **100.0** | **98.7** | **95.2** |

**关键发现**: ORCA 在所有 10 个任务上超越 baseline，DMC Mean 提升 +6.0 vs TADP，MetaWorld Mean 提升 +2.1

### Table 2: Adroit 结果

| Methods | Backbone | Pen | Relocate | Mean |
|---------|----------|-----|----------|------|
| CLIP | ViT-L/16 | 58.7 | 44.0 | 51.4 |
| VC-1 | ViT-L/16 | 65.3 | 29.3 | 47.3 |
| SCR | SD 1.5 | 84.0 | 32.0 | 58.0 |
| TADP | SD 1.5 | 81.3 | 33.3 | 57.3 |
| **ORCA** | SD 1.5 | **86.7** | **44.0** | **65.3** |

**关键发现**: ORCA 在 Adroit dexterous manipulation 任务上 Mean +7.3 vs SCR

### Table 3: 与 Fine-tuning 对比

| Methods | # params. | Pen | Relocate | Mean |
|---------|-----------|-----|----------|------|
| VC-1 | - | 65.3 | 29.3 | 47.3 |
| + Fine-tuning | 302.3M | 58.7 | 4.0 | 31.3 |
| + RoboAdapter | 18.0M | 77.3 | 41.3 | 59.3 |
| SCR | - | 84.0 | 32.0 | 58.0 |
| + Fine-tuning | 346.7M | 17.3 | 1.3 | 9.3 |
| + LoRA | 4.6M | 77.3 | 42.7 | 60.0 |
| **ORCA** | **10.6M** | **86.7** | **44.0** | **65.3** |

**关键发现**: 
- Full fine-tuning 导致性能崩溃（SCR 从 58.0 → 9.3）
- ORCA 参数更少（10.6M vs LoRA 4.6M 近似）但性能更好

### Table 4: 消融实验（Task Prompt + Visual Prompt）

| $p_t$ | $p_v$ | Stand | Walk | Reacher | Cheetah | Finger | Mean |
|-------|-------|-------|------|---------|---------|--------|------|
| | | 85.5 | 64.3 | 81.8 | 43.4 | 66.6 | 68.3 |
| ✓ | | 83.6 | 71.4 | 86.7 | 38.9 | 68.2 | 69.8 |
| | ✓ | 85.9 | 71.1 | 87.3 | 42.0 | 66.1 | 70.5 |
| ✓ | ✓ | **89.1** | **76.9** | **87.6** | **50.0** | **68.0** | **74.3** |

**关键发现**: Task prompt 和 Visual prompt 单独使用时在不同任务表现各异，组合使用在所有任务上一致提升

---

## 实验

### 数据集

| 数据集 | 任务数 | 特点 | 用途 |
|--------|--------|------|------|
| DeepMind Control | 5 | 连续控制，模拟机器人 | 模仿学习，normalized score |
| MetaWorld | 5 | Sawyer 机械臂操作 | 模仿学习，success rate |
| Adroit | 2 | 28-DoF 人手灵巧操作 | 模仿学习，success rate |

### 实现细节

- **Diffusion Backbone**: Stable Diffusion v1.5
- **Visual Encoder**: DINOv2（预训练）
- **Timestep**: $t=0$（不添加噪声）
- **Task tokens**: $l_t=4$
- **Visual tokens**: $l_v=16$
- **Learnable params**: 10.6M（policy + prompts + compression layer）
- **Training**: 100 epochs，每 10 epochs 在线评估
- **Demonstrations**: Adroit 2 条，DMC 5 条，MetaWorld 5 条

---

## 批判性思考

### 优点

1. **简单有效**: 仅用 learnable prompts 替代文本，无需额外优化步骤（vs TADP 的 Textual Inversion）
2. **参数高效**: 10.6M params，远少于 fine-tuning（346.7M）
3. **全面验证**: 12 个任务、多维度对比（task-agnostic、task-adaptive、fine-tuning）

### 局限性

1. **timestep 选择**: 固定 $t=0$，未探索不同 timestep 的效果
2. **仅限模仿学习**: 未验证在 RL、real-world 机器人上的效果
3. **代码未开源**: "Coming Soon"，影响复现性

### 潜在改进方向

1. 探索不同 timestep 或 multi-timestep 特征融合
2. 扩展到 real-world 机器人控制
3. 与其他 backbone（SDXL, SD3）的兼容性验证

### 可复现性评估

- [ ] 代码开源（Coming Soon）
- [x] 实现细节完整
- [x] 训练参数明确
- [x] 数据集公开可用

---

## 关联笔记

### 基于

- [[Stable Diffusion]]: Backbone
- [[DINOv2]]: Visual encoder for visual prompts
- [[SCR]]: Diffusion for control 的先驱工作
- [[TADP]]: 条件化方法的对比 baseline

### 对比

- [[VC-1]]: Task-agnostic 视觉表征 baseline
- [[CLIP]]: 传统视觉表征 baseline
- [[CoOp]]: Learnable prompt 方法（但固定任务名）

### 方法相关

- [[Task Prompt]]: 核心方法 - 任务自适应条件
- [[Visual Prompt]]: 核心方法 - 帧级视觉细节
- [[Cross-Attention]]: 条件注入机制
- [[Behavior Cloning]]: 训练目标

### 应用场景

- [[Imitation Learning]]: 主要应用
- [[Robotic Control]]: 目标任务
- [[Visual Representation]]: 核心问题

---

## 速查卡片

> [!summary] ORCA: Exploring Conditions for Diffusion Models in Robotic Control
> - **核心**: Learnable task prompts + visual prompts 替代文本条件
> - **方法**: DINOv2 dense features → visual tokens, cross-attention 注入 SD U-Net
> - **结果**: DMC 74.3 (+6.0), MetaWorld 95.2 (+2.1), Adroit 65.3 (+7.3)
> - **代码**: https://github.com/ (Coming Soon)

---

*笔记创建时间: 2026-04-13*