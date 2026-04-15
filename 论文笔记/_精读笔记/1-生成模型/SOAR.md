---
title: "SOAR: Self-Correction for Optimal Alignment and Refinement in Diffusion Models"
method_name: "SOAR"
authors: [You Qin, Linqing Wang, Hao Fei, Roger Zimmermann, Liefeng Bo, Qinglin Lu, Chunyu Wang]
year: 2026
venue: arXiv
tags: [diffusion-model, post-training, trajectory-correction, exposure-bias, flow-matching, text-to-image]
zotero_collection: 1-生成模型/Diffusion
image_source: local  # online（默认）/ mixed / local
arxiv_html: https://arxiv.org/html/2604.12617
created: 2026-04-15
---

# 论文笔记：SOAR: Self-Correction for Optimal Alignment and Refinement in Diffusion Models

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Tencent Hunyuan |
| 日期 | April 2026 |
| 项目主页 | - |
| 对比基线 | [[Flow-GRPO]], [[Diffusion-DPO]] |
| 链接 | [arXiv](https://arxiv.org/abs/2604.12617) / [PDF](https://arxiv.org/pdf/2604.12617) |

---

## 一句话总结

> SOAR 提出一种轨迹修正方法，通过构造模型自身产生的偏离轨迹状态并监督其回归正确目标，解决扩散模型的曝光偏差问题，无需奖励模型即可超越 SFT 和 RL 方法。

---

## 核心贡献

1. **曝光偏差识别**: 首次系统性地识别扩散模型中的曝光偏差问题——训练状态与推理状态分布不匹配导致误差累积
2. **SOAR 方法**: 提出轨迹修正算法，通过单步 ODE rollout 构造偏离轨迹状态，重加噪后监督模型回归原始干净目标
3. **无奖励模型优化**: 在精选数据上，SOAR 无需奖励模型即可超越显式 RL 方法（Flow-GRPO），实现单调奖励提升且无 reward hacking

---

## 问题背景

### 要解决的问题

扩散模型的 post-training 流程存在两个阶段：SFT（监督微调）和 RL（强化学习），但两者之间存在根本性差距。当前扩散模型持续出现物体计数错误、手部畸形、文本渲染失败、身份保持不稳定等问题。

### 现有方法的局限

**SFT 的局限**:
- 仅在 ground-truth 状态（前向加噪过程产生的状态）上优化去噪器
- 推理时模型基于自身预测进行条件化，早期误差会传播到未被优化的区域
- 后续去噪依赖 out-of-distribution 泛化而非学习到的修正机制
- 存在与自回归模型相同的曝光偏差，但累积在去噪轨迹而非 token 序列

**RL 的局限**:
- 奖励信号稀疏且终端化，仅在完整 rollout 后可用
- 需要跨数十个去噪步骤进行信用分配，困难
- 过激的单奖励优化可能导致 reward hacking（牺牲多样性或其他质量维度）

### 本文的动机

作者认为这些失败有共同根源：去噪轨迹中的曝光偏差。与其在完整 rollout 后用奖励模型评分，SOAR 直接在偏差发生的 timestep 教模型如何修正自身轨迹错误。

---

## 方法详解

### 模型架构

SOAR 采用 **轨迹修正** 架构，基于 [[Flow Matching]] 框架：

- **输入**: 干净 latent $z_0$ + 条件 $c$ + 噪声 $z_1$
- **Backbone**: [[Rectified Flow]] velocity field $v_\theta(z_t, c, t)$
- **核心模块**: [[Exposure Bias]] 修正通过偏离轨迹状态构造与监督
- **输出**: 修正后的 velocity 预测
- **兼容性**: 可作为 SFT 替代，与后续 RL 对齐完全兼容

### 核心模块

#### 模块1: 偏离轨迹状态构造

**设计动机**: 让模型看到自身推理时实际遇到的状态分布

**具体实现**:
1. 从训练对 $(z_0, c)$ 和噪声 $z_1 \sim \mathcal{N}(0, I)$ 构造 on-trajectory 状态：
   $$z_{\sigma_{t_0}} = (1 - \sigma_{t_0}) z_0 + \sigma_{t_0} z_1$$

2. 使用当前模型进行单步 stop-gradient ODE rollout（带 [[Classifier-Free Guidance]]）：
   $$v_{cfg} = \text{sg}[v_{\text{uncond}} + w_{cfg}(v_{\text{cond}} - v_{\text{uncond}})]$$
   $$\hat{z}_{\sigma_{t_1}} = z_{\sigma_{t_0}} + (\sigma_{t_1} - \sigma_{t_0}) v_{cfg}$$

3. 对偏离状态重加噪，采样 $N$ 个辅助噪声级别 $\sigma_{t'} \sim \text{Uniform}[\sigma_{t_1}, 1]$：
   $$\alpha = \frac{\sigma_{t'} - \sigma_{t_1}}{1 - \sigma_{t_1}}$$
   $$z_{\sigma_{t'}} = (1 - \alpha) \hat{z}_{\sigma_{t_1}} + \alpha z_1$$

**关键设计**: 重加噪使用**相同的 $z_1$**，保持辅助状态在原始传输路径 $z_0 \leftrightarrow z_1$ 的邻域内

#### 模块2: 修正目标推导

**设计动机**: 从干净端点一致性推导修正方向，消除步长歧义

**修正目标推导**:
- 理想轨迹上，状态 $z_\sigma$ 和 velocity $v_{gt} = z_1 - z_0$ 满足：$z_\sigma - \sigma \cdot v_{gt} = z_0$
- 对偏离状态施加相同的目标一致性条件：$z'_\sigma - \sigma \cdot v_{corr} = z_0$
- 解得闭式修正目标：
$$v_{corr} = \frac{z'_\sigma - z_0}{\sigma}$$

**关键性质**:
- 当 $z'_\sigma = z_\sigma$（无偏差）时，$v_{corr} = v_{gt}$，恢复标准 flow matching 目标
- 当 $z'_\sigma \neq z_\sigma$ 时，$v_{corr} - v_{gt} = \frac{z'_\sigma - z_\sigma}{\sigma}$，修正方向与偏离成正比

#### 模块3: 总体损失函数

**损失组合**:
$$\mathcal{L}_{SOAR} = \frac{\mathcal{L}_{base} + \lambda \mathcal{L}_{corr}}{B + \lambda P}$$

其中：
- $\mathcal{L}_{base}$: on-trajectory 基础损失（标准 flow matching）
- $\mathcal{L}_{corr}$: 偏离轨迹修正损失
- $B$: batch 大小，$P$: 辅助点总数
- $\lambda$: 修正损失权重

---

## 关键公式

### 公式1: [[Flow Matching]] 目标

$$
\mathcal{L}_{FM}(\theta) = \mathbb{E}_{z_0, z_1, c, t} \|v_\theta(z_t, c, t) - (z_1 - z_0)\|^2
$$

**含义**: 标准 flow matching 损失，训练 velocity field 预测从噪声到数据的传输方向

**符号说明**:
- $z_0$: 干净 latent，从数据分布采样
- $z_1 \sim \mathcal{N}(0, I)$: 高斯噪声
- $z_t = (1-t)z_0 + tz_1$: 线性插值路径上的状态
- $v_\theta(z_t, c, t)$: 神经网络预测的 velocity

### 公式2: [[Rectified Flow]] 线性插值路径

$$
z_t = (1 - t) z_0 + t z_1, \quad t \in [0, 1]
$$

**含义**: 定义从数据到噪声的直线路径，每对 $(z_0, z_1)$ 定义一条传输路径

**符号说明**:
- $t$: 时间参数，索引噪声级别
- $\sigma_t$: 对应的 scheduler 噪声级别

### 公式3: Euler 离散采样

$$
z_{t-\Delta t} = z_t + \Delta t \cdot v_\theta(z_t, c, t), \quad \Delta t = -1/K
$$

**含义**: 从 $t=1$（纯噪声）到 $t=0$（干净数据）的 ODE 解

**符号说明**:
- $K$: 总采样步数
- $\Delta t$: 每步时间增量

### 公式4: 干净端点估计

$$
\hat{z}_0(z, \sigma) = z - \sigma \cdot v_\theta(z, c, \sigma)
$$

**含义**: 从当前状态和 velocity 预测推导干净 latent

**符号说明**:
- $\hat{z}_0$: 模型预测的干净端点
- $\sigma$: 当前噪声级别

### 公式5: [[Exposure Bias]] 分布定义

$$
p_\sigma^{\text{data}}(z) = \int \delta((1-\sigma)z_0 + \sigma z_1)(z) \, q(z_0) \mathcal{N}(z_1) \, dz_0 dz_1
$$

**含义**: SFT 训练状态的 ground-truth 分布，由前向过程产生

**符号说明**:
- $p_\sigma^{\text{data}}$: 噪声级别 $\sigma$ 下的理想状态分布
- $\delta$: Dirac delta 函数

### 公式6: 偏离轨迹状态构造

$$
\hat{z}_{\sigma_{t_1}} = z_{\sigma_{t_0}} + (\sigma_{t_1} - \sigma_{t_0}) v_{cfg}
$$

**含义**: 单步 ODE rollout 产生的偏离轨迹状态

**符号说明**:
- $\hat{z}_{\sigma_{t_1}}$: 模型自身预测的状态（偏离理想轨迹）
- $v_{cfg}$: stop-gradient CFG velocity

### 公式7: 辅助状态重加噪

$$
z_{\sigma_{t'}} = (1 - \alpha) \hat{z}_{\sigma_{t_1}} + \alpha z_1
$$

**含义**: 将偏离状态重新加噪到辅助噪声级别

**符号说明**:
- $\alpha = \frac{\sigma_{t'} - \sigma_{t_1}}{1 - \sigma_{t_1}}$: 插值系数
- $\sigma_{t'}$: 辅助噪声级别

### 公式8: [[Trajectory Correction]] 目标

$$
v_{corr} = \frac{z'_\sigma - z_0}{\sigma}
$$

**含义**: SOAR 的核心修正目标，引导偏离状态回归正确干净端点

**符号说明**:
- $v_{corr}$: 修正 velocity 目标
- $z'_\sigma$: 偏离轨迹状态
- $z_0$: 原始干净 latent（唯一正确的目标）

### 公式9: SOAR 总损失

$$
\mathcal{L}_{SOAR} = \frac{\sum_{b=1}^B \ell_{base}^{(b)} + \lambda \sum_{p=1}^P \ell_{aux}^{(p)}}{B + \lambda P}
$$

**含义**: 组合 on-trajectory 和 off-trajectory 损失，归一化确保梯度稳定

**符号说明**:
- $\ell_{base}$: 基础损失项
- $\ell_{aux}$: 辅助修正损失项
- $\lambda$: 修正权重系数

---

## 关键图表

### Figure 1: Post-training 范式概念对比

![[../assets/SOAR/SOAR-002.png]]

**说明**: 从轨迹视角对比三种 post-training 范式。左：SFT 仅在 ground-truth 去噪状态上训练；中：RL 通过终端奖励区分好坏最终结果，信号稀疏；右：SOAR 执行单步 rollout 生成偏离轨迹状态，监督模型在语义错误完全形成前修正。

### Figure 2: 噪声级别分析

![[../assets/SOAR/SOAR-004.png]]

**说明**: 展示理想轨迹、偏差 rollout 和局部修正方向。偏离状态由当前模型单步 ODE 产生，然后重加噪到辅助级别。

### Figure 3: 奖励特定训练动态

![[../assets/SOAR/SOAR-196.png]]

**说明**: 512x512 分辨率下的奖励特定训练动态。(a) OOD DrawBench Aesthetic score，在 3725 对高美学子集（>=6.8）上训练；(b) OOD DrawBench ClipScore，在 6857 对高 ClipScore 子集上训练。SOAR 在两种设置下均达到最高最终值且单调改进。

### Figure 4: 高美学数据训练可视化

![[../assets/SOAR/SOAR-200.png]]

**说明**: 高美学子集的训练可视化，覆盖肖像、风景和动漫风格。随着训练进展，SOAR 快速获得连贯构图和结构保真度。

### Figure 5: Poster 数据训练可视化

![[../assets/SOAR/SOAR-210.png]]

**说明**: Poster 数据训练可视化。SOAR 学习布局结构、排版位置和视觉层级，保真度高。

### Figure 6: WebUI 数据训练可视化

![[../assets/SOAR/SOAR-220.png]]

**说明**: WebUI 数据训练可视化。SOAR 准确捕捉界面元素、阴影效果和细粒度风格细节。

### Table 1: Post-training 范式定性对比

| 方法 | 训练信号 | On-policy | 信号密度 | 需奖励模型 | 信用分配问题 | Reward Hacking | 训练成本 |
|------|----------|-----------|----------|------------|--------------|----------------|----------|
| SFT | 监督(GT轨迹) | No | Dense | No | No | No | Low |
| DPO | 偏好对(隐式奖励) | No | Sparse | Yes | No | Partial | Medium |
| RL | 奖励函数 | Yes | Sparse | Yes | Yes | Yes | High |
| **SOAR** | 监督(轨迹修正) | Yes(单步) | Dense | No | No | No | Low |

**说明**: SOAR 结合 on-policy 训练与稠密无奖励监督，同时保持低训练成本。

### Table 2: 主量化对比（512x512, cfg=4.5）

| Model | #Iter | GenEval | OCR | PickScore | ClipScore | HPSv2.1 | Aesthetic | ImageReward |
|-------|-------|---------|-----|-----------|-----------|---------|-----------|-------------|
| SD-XL | - | 0.55 | 0.14 | 22.42 | 0.287 | 0.280 | 5.60 | 0.76 |
| SD3.5-L | - | 0.71 | 0.68 | 22.91 | 0.289 | 0.288 | 5.50 | 0.96 |
| FLUX.1-Dev | - | 0.66 | 0.59 | 22.84 | 0.295 | 0.274 | 5.71 | 0.96 |
| SD3.5-M | - | 0.63 | 0.59 | 22.34 | 0.285 | 0.279 | 5.36 | 0.85 |
| + SFT | 10k | 0.70 | 0.64 | 22.71 | 0.295 | 0.284 | 5.35 | 1.04 |
| + **SOAR** | 10k | **0.78** | **0.67** | **22.86** | **0.295** | **0.289** | **5.46** | **1.09** |

**说明**: SOAR 在所有指标上超越 SFT：GenEval +11% 相对提升（0.70→0.78），OCR +4.7%，所有模型偏好分数同时提升。无需奖励模型即可实现。

### Table 3: SOAR 设计消融

| Variant | GenEval | OCR | PickScore | ClipScore | HPSv2.1 | Aesthetic | ImageReward |
|---------|---------|-----|-----------|-----------|---------|-----------|-------------|
| ODE-only, shared z1 | **0.78** | **0.67** | 22.86 | **0.295** | **0.289** | **5.46** | **1.09** |
| + SDE branch | 0.78 | 0.65 | 22.87 | 0.296 | 0.287 | 5.47 | 1.07 |
| + random z1 per path | 0.76 | 0.64 | **22.89** | 0.292 | 0.287 | 5.45 | 1.08 |

**关键发现**: ODE-only + 共享 z1 是最佳默认配置；移除 SDE 分支略微改善 GenEval、OCR、HPSv2.1；共享 z1 保持辅助状态在原始传输路径邻域内，确保 z0 作为正确修正锚点。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Full corpus | 286,119 对 | 广泛图像-文本数据 | 宽泛对比训练 |
| High-Aesthetic | 3,725 对 | aesthetic >= 6.8 | 奖励特定实验 |
| High-ClipScore | 6,857 对 | CLIP score >= 0.40 | 奖励特定实验 |

### 实现细节

- **Backbone**: SD3.5-Medium
- **分辨率**: 512x512, cfg=4.5
- **优化器**: 常数学习率 2e-5
- **Batch Size**: 512（宽泛对比），32（奖励特定）
- **训练步数**: 10k（宽泛），5k（奖励特定）
- **硬件**: 128 GPUs（奖励特定实验）

### 关键实验发现

1. **宽泛数据对比**: SOAR 在所有指标上超越 SFT，GenEval +11% 相对提升
2. **奖励特定训练**: 在精选数据上，SOAR 无需奖励模型即可超越 Flow-GRPO
3. **单调改进**: SOAR 每个 checkpoint 都优于上一个，无 reward hacking
4. **RL 局限**: GRPO 改善目标 aesthetic 时 ClipScore 下降 2.8%（reward hacking）

---

## 批判性思考

### 优点

1. **理论清晰**: 从曝光偏差角度统一解释多种生成失败模式
2. **无奖励模型**: 在精选数据上实现偏好对齐，无需外部奖励模型
3. **稠密监督**: 修正信号在偏差发生的 timestep 提供，无信用分配问题
4. **兼容性好**: 可直接替换 SFT 作为 post-training 第一阶段，与后续 RL 兼容
5. **单调改进**: 无 reward hacking 风险，所有指标同时改善

### 局限性

1. **多样性影响**: 轨迹修正可能降低高噪声级别的生成多样性（需定量评估）
2. **噪声权重**: 当前使用继承的 flow matching 权重，专用权重设计可能进一步改善
3. **单步 rollout**: 仅使用单步偏离，多步偏离情况未探索
4. **数据质量依赖**: 修正目标锚定干净样本，低质量数据会注入错误监督

### 潜在改进方向

1. **多样性评估**: 用 LPIPS 方差和 Vendi Score 定量评估多样性权衡
2. **噪声权重设计**: 专用权重调度，如低 σ 降权防止梯度爆炸
3. **多步 rollout**: 探索 K>1 步偏离轨迹修正
4. **扩展应用**: 视频、3D 合成和模型蒸馏

### 可复现性评估

- [x] 方法完整描述（Algorithm 1 详细伪代码）
- [x] 训练细节完整
- [ ] 代码开源（未提及）
- [ ] 预训练模型
- [x] 数据集描述（规模、筛选标准）
- [x] 评估协议完整

---

## 关联笔记

### 基于

- [[Flow Matching]]: 理论基础，rectified flow velocity field
- [[Rectified Flow]]: 线性插值路径架构
- [[Diffusion Model]]: 生成模型框架
- [[Classifier-Free Guidance]]: rollout 时使用的条件引导

### 对比

- [[Flow-GRPO]]: RL 方法对比，SOAR 在精选数据上无需奖励模型即可超越
- [[Diffusion-DPO]]: 离线偏好学习方法对比
- [[SFT]]: 标准 post-training 对比，SOAR 直接替换 SFT

### 方法相关

- [[Exposure Bias]]: 核心问题识别
- [[Trajectory Correction]]: SOAR 核心方法
- [[Reward Hacking]]: RL 方法风险，SOAR 避免
- [[Credit Assignment]]: RL 方法困难，SOAR 无此问题

### 硬件/数据相关

- [[SD3.5]]: 实验 backbone
- [[GenEval]]: 组合准确性评估指标
- [[PickScore]]: 模型偏好评估指标

---

## 速查卡片

> [!summary] SOAR
> - **核心**: 轨迹修正解决扩散模型曝光偏差
> - **方法**: 单步 ODE rollout 构造偏离状态 + 重加噪 + 监督回归干净目标
> - **结果**: GenEval 0.70→0.78，无需奖励模型超越 RL
> - **特点**: On-policy + Dense + Reward-free + Low cost

---

*笔记创建时间: 2026-04-15*