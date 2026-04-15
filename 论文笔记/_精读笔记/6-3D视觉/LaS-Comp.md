---
title: "LaS-Comp: Zero-shot 3D Completion with Latent-Spatial Consistency"
method_name: "LaS-Comp"
authors: [Weilong Yan, Haipeng Li, Hao Xu, Nianjin Ye, Yihao Ai, Shuaicheng Liu, Jingyu Hu]
year: 2026
venue: CVPR 2026
tags: [shape-completion, 3d-foundation-model, zero-shot, latent-generation, point-cloud]
zotero_collection: 6-3D视觉
image_source: mixed
arxiv_html: https://arxiv.org/html/2602.18735v2
created: 2026-04-13
---

# 论文笔记：LaS-Comp: Zero-shot 3D Completion with Latent-Spatial Consistency

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | National University of Singapore / University of Electronic Science and Technology of China / The Chinese University of Hong Kong / Changhong Intelligent Robot |
| 日期 | February 2026 |
| 项目主页 | https://github.com/DavidYan2001/LaS-Comp |
| 对比基线 | [[SDS-Complete]], [[ComPC]], [[GenPC]] |
| 链接 | [arXiv](https://arxiv.org/abs/2602.18735) / [Code](https://github.com/DavidYan2001/LaS-Comp) |

---

## 一句话总结

> 提出零样本、类别无关的 3D 形状补全框架，通过显式替换和隐式对齐两阶段设计，弥合 latent 与 spatial 域之间的 gap，实现高保真补全。

---

## 核心贡献

1. **LaS-Comp 框架**: 首个完全利用 latent-generative-based [[3D Foundation Model]] 的零样本形状补全框架，通过两阶段设计（ERS + IAS）弥合 latent-spatial gap
2. **训练自由 (Training-free)**: 无需配对数据训练，兼容多种 3D foundation models，单次补全仅需 20 秒（比现有 zero-shot 方法快 3 倍以上）
3. **Omni-Comp Benchmark**: 提出综合评估基准，结合真实扫描与合成数据，涵盖多种 partial patterns（random crops、single-view scans、semantic parts missing）

---

## 问题背景

### 要解决的问题
从部分观测的 3D 点云 $\boldsymbol{S}_p \in \mathbb{R}^{k \times 3}$ 生成完整的 3D 形状 $\boldsymbol{S}_c$，要求：
- 处理多样的 partial patterns（单视角扫描到语义部件缺失）
- 跨类别泛化
- 无需配对的 partial-complete 数据集
- 支持文本引导和无条件补全

### 现有方法的局限
1. **监督学习方法**（如 SVDFormer、AdaPoinTr）依赖配对数据集，泛化能力受限
2. **无监督方法**（如 P2C、Shape-Inv）在 ShapeNet 上训练，类别有限，真实场景表现差
3. **生成先验方法**（如 SDS-Complete、ComPC、GenPC）假设 partial input 可渲染成至少一张完整图像，当缺失区域无法从任何视角隐藏时失效

### 本文的动机
现代 [[3D Foundation Model]]（如 [[DORA]]）在大规模数据上训练，具有强大的跨类别生成能力。但这些模型采用 latent-generative pipeline：VAE 编码到紧凑 latent space，再由 diffusion/flow-matching 模型生成。问题在于：**即使完整形状和 partial input 在重叠区域几何相同，它们的 latent 编码也差异显著**，导致直接在 latent space 补全不可靠。

---

## 方法详解

### 模型架构

LaS-Comp 采用 **latent-generative** 架构：

- **输入**: Partial 点云 $\boldsymbol{S}_p$ + optional 文本指令
- **Backbone**: [[DORA]] (3D foundation model，VAE encoder + Flow Matching generator)
- **核心模块**: [[ERS]]（显式替换）+ [[PNS]]（Partial-aware Noise Schedule）+ [[IAS]]（隐式对齐）
- **输出**: Complete 点云 $\boldsymbol{S}_c$
- **耗时**: ~20 seconds per shape

### 整体流程

从 Gaussian noise $\boldsymbol{x}_1$ 开始，迭代 denoising，每步包含：
1. **ERS**: 显式注入 partial geometry 到 latent，保证 fidelity
2. **IAS**: 梯度优化 refine latent，确保 boundary coherence

最终 decode latent 得到 complete shape。

### 核心模块

#### 模块1: Explicit Replacement Stage (ERS)

**设计动机**: 弥合 latent 与 spatial 域的 gap，通过 spatial-domain replacement 确保输入保真度

**双分支设计**（借鉴 [[FlowDPS]]）:

- **Clean Branch**: 估计 noise-free latent $\hat{\boldsymbol{x}}_{0|t}$，decode 到 spatial domain $\boldsymbol{S}_{0|t}$，用 mask $\boldsymbol{M}$ 替换观测区域，再 encode 回 latent $\boldsymbol{x}^*_{0|t}$

- **Noisy Branch**: 估计 noisy latent $\hat{\boldsymbol{x}}_{1|t}$，通过 [[PNS]] 调制 stochasticity：
  - 观测区域：混合预测 latent 和噪声（早迭代允许更多调整，晚迭代趋于稳定）
  - 缺失区域：纯噪声（鼓励探索多样性）

**最终 latent**: $\boldsymbol{x}^*_t = (1-t) \cdot \boldsymbol{x}^*_{0|t} + t \cdot \boldsymbol{x}^*_{1|t}$

#### 模块2: Implicit Alignment Stage (IAS)

**设计动机**: 仅 ERS 不够，需进一步优化 latent 使 boundary coherent

**具体实现**:
- Decode $\boldsymbol{x}^*_t$ 得到 $\boldsymbol{S}_{0|t}$
- 计算 [[BCE Loss]] 对齐观测区域
- 梯度优化一步：$\boldsymbol{x}^{\text{aligned}}_{0|t} = \hat{\boldsymbol{x}}_{0|t} - \eta \cdot \nabla_{\hat{\boldsymbol{x}}_{0|t}} \mathcal{L}_{\text{align}}$
- 加噪到下一 timestep

---

## 关键公式

### 公式1: [[Flow Matching|Noise-free Latent Estimation]]

$$
\hat{\boldsymbol{x}}_{0|t} = \boldsymbol{x}_{t} - t \cdot \mathcal{G}(\boldsymbol{x}_{t}, t)
$$

**含义**: 基于 linear flow path 估计 $t$ 时刻的 noise-free latent

**符号说明**:
- $\boldsymbol{x}_t$: 当前 latent feature ($t \in [0, 1]$)
- $\mathcal{G}(\boldsymbol{x}_t, t)$: Generator 预测的速度场（velocity）
- $t$: 当前时间步

### 公式2: Spatial Decoding

$$
\boldsymbol{S}_{0|t} = \mathcal{D}(\hat{\boldsymbol{x}}_{0|t})
$$

**含义**: Decode latent 到 spatial domain 得到 shape prediction

**符号说明**:
- $\mathcal{D}$: VAE Decoder
- $\boldsymbol{S}_{0|t} \in \mathbb{R}^{N^3}$: Occupancy grid

### 公式3: [[Spatial Replacement]]

$$
\boldsymbol{S}'_{0|t} = \boldsymbol{S}_{p} \odot \boldsymbol{M} + \boldsymbol{S}_{0|t} \odot (1 - \boldsymbol{M})
$$

**含义**: 观测区域用 partial input 替换，缺失区域用生成预测填充

**符号说明**:
- $\boldsymbol{S}_p$: Partial input（voxelized）
- $\boldsymbol{M} \in \{0, 1\}^{N^3}$: Binary mask（观测区域为 1）
- $\odot$: Element-wise multiplication

### 公式4: Re-encoding

$$
\boldsymbol{x}^*_{0|t} = \mathcal{E}(\boldsymbol{S}'_{0|t})
$$

**含义**: Encode 替换后的 shape 回 latent space

**符号说明**:
- $\mathcal{E}$: VAE Encoder
- $\boldsymbol{x}^*_{0|t}$: 包含 partial geometry 的 latent

### 公式5: Noisy Latent Estimation

$$
\hat{\boldsymbol{x}}_{1|t} = \boldsymbol{x}_{t} + (1 - t) \cdot \mathcal{G}(\boldsymbol{x}_{t}, t)
$$

**含义**: 估计 $t$ 时刻的 noisy latent（对应纯噪声端）

### 公式6: [[PNS|Partial-aware Noise Schedule]]

$$
\boldsymbol{x}^*_{1|t} = \boldsymbol{M} \odot \left(\sqrt{1-t} \cdot \hat{\boldsymbol{x}}_{1|t} + \sqrt{t} \cdot \boldsymbol{\epsilon}_1\right) + (1 - \boldsymbol{M}) \odot \boldsymbol{\epsilon}_2
$$

**含义**: 观测区域混合预测和噪声，缺失区域纯噪声

**符号说明**:
- $\boldsymbol{\epsilon}_1, \boldsymbol{\epsilon}_2 \sim \mathcal{N}(\boldbf{0}, \boldsymbol{I})$: Gaussian noise
- $\sqrt{1-t}, \sqrt{t}$: Time-dependent coefficients（早迭代噪声大，晚迭代噪声小）

### 公式7: Latent Interpolation

$$
\boldsymbol{x}^*_{t} = (1 - t) \cdot \boldsymbol{x}^*_{0|t} + t \cdot \boldsymbol{x}^*_{1|t}
$$

**含义**: 线性插值 clean 和 noisy latent

### 公式8: IAS Noise-free Estimation

$$
\hat{\boldsymbol{x}}_{0|t} = \boldsymbol{x}^*_{t} - t \cdot \mathcal{G}(\boldsymbol{x}^*_{t}, t)
$$

**含义**: 对 ERS 输出的 latent 做 noise-free estimation

### 公式9: IAS Spatial Decoding

$$
\boldsymbol{S}_{0|t} = \mathcal{D}(\hat{\boldsymbol{x}}_{0|t})
$$

### 公式10: [[Alignment Loss]]

$$
\mathcal{L}_{\text{align}} = \text{BCE}(\boldsymbol{S}_{0|t} \odot \boldsymbol{M}, \boldsymbol{S}_{p} \odot \boldsymbol{M})
$$

**含义**: Binary Cross Entropy 损失，对齐观测区域

**符号说明**:
- BCE: Binary Cross Entropy
- 仅在 mask 区域计算损失

### 公式11: Gradient-based Alignment

$$
\boldsymbol{x}^{\text{aligned}}_{0|t} = \hat{\boldsymbol{x}}_{0|t} - \eta \cdot \nabla_{\hat{\boldsymbol{x}}_{0|t}} \mathcal{L}_{\text{align}}
$$

**含义**: 一步梯度优化 refine latent

**符号说明**:
- $\eta$: Learning rate

### 公式12: Next Timestep

$$
\boldsymbol{x}_{t-dt} = \boldsymbol{x}^{\text{aligned}}_{0|t} + (t - dt) \cdot \mathcal{G}(\boldsymbol{x}^*_{t}, t)
$$

**含义**: 加噪到下一 timestep 继续迭代

---

## 关键图表

### Figure 1: Teaser / 系统概览

![LaS-Comp_fig1](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig1.png)

**说明**: LaS-Comp 支持多样 partial patterns：(a) random crops、(b) single-view scans、(c) semantic parts missing。同时支持 (d) 文本引导和无条件补全。

### Figure 2: Pipeline Overview / 流程概览

![LaS-Comp_fig2](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig2.png)

**说明**: 从 Gaussian noise 开始，迭代 refine latent $\boldsymbol{x}_t$。每步包含 ERS（显式替换）和 IAS（隐式对齐）。

### Figure 3: ERS Architecture / 显式替换阶段架构

![LaS-Comp_fig3](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig3.png)

**说明**: ERS 双分支设计。Clean Branch（上）通过 spatial replacement 确保 fidelity；Noisy Branch（下）通过 PNS 调制 stochasticity。最终线性插值得到 $\boldsymbol{x}^*_t$。

### Figure 4: Redwood Comparison / 真实扫描对比

![LaS-Comp_fig4](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig4.png)

**说明**: 在真实 Redwood 数据上的可视化对比，LaS-Comp 生成更完整、细节更丰富的形状。

### Figure 5: KITTI & ScanNet Comparison / 场景扫描对比

![LaS-Comp_fig5](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig5.png)

**说明**: 在 KITTI（自动驾驶）和 ScanNet（室内场景）数据上的对比，验证真实场景泛化能力。

### Figure 6: Omni-Comp Comparison / Omni-Comp Benchmark对比

![LaS-Comp_fig6](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig6.png)

**说明**: 在提出的 Omni-Comp benchmark 上的对比，涵盖 random crops、single scans、semantic parts 三种 partial patterns。

### Figure 7: Diversity / 补全多样性

![LaS-Comp_fig7](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig7.png)

**说明**: 同一 partial input 的多样补全结果，体现生成能力。

### Figure 8: Ablation / 消融实验可视化

![LaS-Comp_fig8](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/LaS-Comp_fig8.png)

**说明**: 各组件贡献可视化。w/o ERS：生成完全不同形状；w/o PNS：条纹伪影；w/o IAS：boundary holes。

### Table 1: Completion Correctness on Redwood

| Method | CD $\downarrow$ / EMD $\downarrow$ | Table | Exe-Chair | Out-Chair | Old-Chair | Vase | Off-Can | Vespa | Tricycle | Trash | Couch | Average |
|--------|-------------------------------------|-------|-----------|-----------|-----------|------|---------|-------|----------|-------|-------|---------|
| SVDFormer | 5.48/6.68 | 3.20/5.85 | 0.79/1.45 | 3.79/5.95 | 5.70/6.98 | 5.13/6.71 | 3.10/4.91 | 2.73/4.96 | 3.67/5.05 | 1.86/2.92 | 3.54/5.15 |
| AdaPoinTr | 5.02/6.25 | 2.58/4.80 | 0.82/1.37 | 3.62/5.64 | 5.14/6.50 | 4.47/6.35 | 1.96/3.54 | 1.83/3.66 | 1.21/3.08 | 1.01/2.09 | 2.77/4.33 |
| PCDreamer | 0.82/2.36 | 1.43/3.56 | 1.19/2.07 | 2.61/5.34 | 2.41/3.64 | 2.62/3.89 | 2.20/4.30 | 2.53/3.82 | 1.55/2.47 | 1.47/2.61 | 1.88/3.41 |
| **LaS-Comp** | **0.78/1.53** | **0.89/1.48** | **0.77/1.21** | **2.12/3.29** | **1.98/3.21** | **1.92/3.12** | **1.42/2.39** | **1.39/2.41** | **1.03/1.87** | **0.91/1.48** | **1.32/2.27** |

**说明**: LaS-Comp 在所有类别上 CD/EMD 指标最优，平均 CD 1.32（相比 PCDreamer 1.88 提升 30%）

### Table 6: Ablation Study

| Configuration | CD $\downarrow$ / EMD $\downarrow$ (Redwood) | CD $\downarrow$ / EMD $\downarrow$ (Omni-Comp) |
|---------------|----------------------------------------------|------------------------------------------------|
| (a) Naive Baseline | 5.32 / 6.87 | 4.89 / 5.62 |
| (b) Full w/o ERS | 3.42 / 4.94 | 3.53 / 4.85 |
| (c) Full w/o PNS | 1.94 / 2.56 | 2.27 / 2.67 |
| (d) Full w/o IAS | 1.88 / 2.14 | 1.17 / 1.56 |
| (e) IAS w/ 10 steps | 1.42 / 1.87 | 1.20 / 1.48 |
| **(f) Full pipeline (Ours)** | **1.42 / 1.84** | **1.11 / 1.41** |

**关键发现**:
- ERS 最关键（移除后 CD 从 1.42 升至 3.42）
- PNS 消除条纹伪影
- IAS 确保 boundary coherence
- IAS 1 步优化足够，10 步无明显提升

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Redwood | 10 categories | 真实扫描，large-scale | Completion Correctness/Fidelity 评估 |
| KITTI | ~80 objects | 自动驾驶场景 | In-the-wild 测试 |
| ScanNet | ~100 objects | 室内场景扫描 | In-the-wild 测试 |
| Omni-Comp (Proposed) | 多种 patterns | Random crops + Single scans + Semantic parts missing | Comprehensive evaluation |

### 实现细节

- **Backbone**: [[DORA]]（VAE encoder/decoder + Flow Matching generator）
- **Denoising Steps**: 100 steps (ODE solver)
- **IAS Optimization**: 1 step per timestep, $\eta = 0.01$
- **Hardware**: NVIDIA A100 GPU
- **耗时**: ~20 seconds per shape
- **Voxel Resolution**: $N = 64$（spatial domain）

### 可视化结果

- 真实扫描（Redwood/KITTI/ScanNet）上生成细节丰富、几何准确的完整形状
- Omni-Comp 上处理三种 partial patterns 均表现优异
- 同一 input 可生成多样补全结果

---

## 批判性思考

### 优点
1. **零样本、类别无关**: 无需配对数据训练，直接利用 foundation model 先验
2. **效率高**: 20 秒完成，比 SDS-Complete/ComPC（需迭代 SDS 优化）快 3 倍以上
3. **泛化能力强**: 处理多样 partial patterns，真实场景表现好
4. **灵活控制**: 支持文本引导和无条件补全

### 局限性
1. **依赖 foundation model**: 需预训练的 3D foundation model（如 DORA），当前此类模型有限
2. **Voxel representation**: 使用 occupancy grid，分辨率受限（$N=64$），可能丢失精细几何
3. **单步优化**: IAS 仅 1 步优化，复杂 boundary 可能需要更多步
4. **物体级别**: 未处理场景级别的 completion

### 潜在改进方向
1. 结合更高分辨率 voxel 或 implicit representation（如 SDF/NeRF）
2. 扩展到场景级别 completion
3. 探索更多 3D foundation models（如 Clay、Michibichi）
4. 多步 IAS 优化处理复杂 boundary

### 可复现性评估
- [x] 代码开源（GitHub: DavidYan2001/LaS-Comp）
- [ ] 预训练模型（待发布）
- [x] 训练细节完整（论文 Appendix）
- [x] 数据集可获取（Omni-Comp 将发布）

---

## 关联笔记

### 基于
- [[DORA]]: 3D foundation model backbone
- [[FlowDPS]]: Latent decomposition 思想启发 ERS 双分支设计
- [[Flow Matching]]: Generator 的生成机制

### 对比
- [[SDS-Complete]]: 2D prior + SDS 优化，假设可渲染完整图像
- [[ComPC]]: 3D Gaussians + Zero-1-to-3，同样依赖渲染
- [[GenPC]]: Image-to-3D + Zero-1-to-3 refinement

### 方法相关
- [[Shape Completion]]: 核心任务
- [[ERS]]: 显式替换阶段
- [[IAS]]: 隐式对齐阶段
- [[PNS]]: Partial-aware Noise Schedule
- [[Latent-Spatial Gap]]: 论文识别的核心问题

### 硬件/数据相关
- [[Omni-Comp]]: 提出的 benchmark
- [[Redwood Dataset]]: 评估数据集

---

## 速查卡片

> [!summary] LaS-Comp
> - **核心**: 零样本 3D 形状补全，弥合 latent-spatial gap
> - **方法**: ERS（显式替换保 fidelity）+ IAS（隐式对齐保 coherence）
> - **结果**: CD 1.32（Redwood），比 zero-shot baseline 快 3x
> - **代码**: https://github.com/DavidYan2001/LaS-Comp

---

*笔记创建时间: 2026-04-13*