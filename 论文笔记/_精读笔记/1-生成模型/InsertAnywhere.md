---
title: "InsertAnywhere: Bridging 4D Scene Geometry and Diffusion Models for Realistic Video Object Insertion"
method_name: "InsertAnywhere"
authors: [Hoiyeong Jin, Hyojin Jang, Jeongho Kim, Junha Hyung, Kinam Kim, Dongjin Kim, Huijin Choi, Hyeonji Kim, Jaegul Choo]
year: 2025
venue: arXiv
tags: [video-object-insertion, diffusion-model, 4d-scene-reconstruction, video-editing, video-inpainting, illumination-aware]
zotero_collection: 1-生成模型/视频生成
image_source: online
arxiv_html: https://arxiv.org/html/2512.17504v1
created: 2026-04-03
---

# 论文笔记：InsertAnywhere: Bridging 4D Scene Geometry and Diffusion Models for Realistic Video Object Insertion

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | KAIST AI, SK Telecom |
| 日期 | 2025年12月 |
| 项目主页 | https://myyzzzoooo.github.io/InsertAnywhere/ |
| 对比基线 | [[KLing]], [[Pika-Pro]] |
| 链接 | [arXiv](https://arxiv.org/abs/2512.17504) |

---

## 一句话总结

> 提出 InsertAnywhere 框架，通过 4D 场景几何重建生成几何一致的遮挡感知 mask，结合扩散模型合成光照变化，实现商业级视频对象插入。

---

## 核心贡献

1. **4D-aware Mask Generation Module**: 通过 [[4D Scene Reconstruction|4D场景重建]] 和场景流传播，从用户指定的首帧位置生成时间一致、遮挡正确的 mask 序列，支持用户交互式 GUI
2. **Illumination-Aware Video Generation**: 扩展 [[Video Inpainting|视频修复]] 模型，联合合成插入对象及其周围的局部光照变化（阴影、照明等）
3. **ROSE++ Dataset**: 将 ROSE 物体移除数据集转换为 triplet 格式（移除视频、原始视频、VLM 生成的参考图），通过任务反转实现监督训练
4. **超越商业工具**: 在 VOIBench 上显著超越 KLing 和 Pika-Pro，达到生产环境可用质量

---

## 问题背景

### 要解决的问题

[[Video Object Insertion|视频对象插入]] (VOI) 需要在现有视频中无缝插入新对象，同时保持空间、时间和光照一致性。主要挑战：
- **几何放置**: 单张参考图无法提供合适的尺度/姿态信息，需要用户控制 + 4D 场景理解
- **遮挡处理**: 动态物体导致的复杂遮挡场景
- **光照一致性**: 插入对象需要与场景光照协调，包括阴影、反射等

### 现有方法的局限

1. **商业工具** ([[KLing]], [[Pika-Pro]]):
   - 无法可靠推断对象尺度和姿态
   - 遮挡处理失败（对象前后关系混乱）
   - 对象保真度低（颜色/结构偏差）

2. **研究方法** ([[GenProp]]):
   - 使用大范围 mask 降低用户可控性
   - 未显式处理可见性变化，遮挡时失败
   - 仅在 mask 内编辑，无法捕获光照变化

3. **传统 Inpainting**:
   - 仅填充 mask 区域，无法生成阴影/光照
   - 自监督训练无光照监督信号

### 本文的动机

作者认为商业级 VOI 需要两个核心能力：
1. **4D-aware Object Placement**: 用户控制 + 几何理解 + 遮挡一致性
2. **High-fidelity Video Generation**: 对象外观 + 局部光照变化联合合成

通过将物体移除任务反转为插入任务，利用 ROSE++ 数据集提供光照监督。

---

## 方法详解

### 模型架构

InsertAnywhere 采用 **两阶段流水线** 架构：

- **输入**: 源视频 $V_{source}$、参考对象图 $I_{obj}$、用户指定的首帧 mask
- **Stage 1**: [[4D-aware Mask Generation]] - 重建 4D 场景，传播对象位置
- **Stage 2**: [[Video Object Insertion]] - 扩散模型合成最终视频
- **输出**: 插入对象后的视频，包含光照变化

### 核心模块

#### Stage 1: 4D-aware Mask Generation

**设计动机**: 用户仅提供首帧位置，需要自动生成时间一致、遮挡正确的 mask 序列

**子模块结构**:

1. **4D Scene Reconstruction** (基于 [[Uni4D]])
   - 整合多个预训练模型：深度估计、光流、相机姿态、分割
   - 构建时间一致的 4D 表示（几何 + 运动）

2. **User-Controlled Object Placement**
   - 对象图 $I_{obj}$ 通过单视图重建网络转换为 3D 点云 $\mathbf{Y}=\{y_{j}\in\mathbb{R}^{3}\}$
   - 用户通过 GUI 控制位置、尺度、姿态参数

3. **Scene Flow based Object Propagation**
   - 使用 [[SEA-RAFT]] 计算密集光流 $\mathbf{V}_{t\rightarrow t+1}$
   - 找到对象周围的 $K$ 个最近 3D 点
   - 聚合局部场景流，传播对象运动

4. **Camera-Aligned Reprojection**
   - 将 3D 点投影到各帧图像平面
   - 使用 [[SAM2]] 提取二值 mask 序列 $\{M_{t}\}_{t=1}^{T}$

#### Stage 2: Video Object Insertion

**设计动机**: 利用图像模型的高保真度作为首帧锚点，传播到后续帧

**具体实现**:
- 基于视频扩散模型 [[Wan2.1-VACE]]，使用 [[LoRA]] 微调
- 首帧使用图像 inpainting 模型生成高保真结果
- 视频模型基于首帧传播外观信息
- 在 ROSE++ 上训练，学习光照/阴影生成

---

## 关键公式

### 公式1: [[Object Placement|对象点云变换]]

$$
y^{\prime}_{j}=s_{\text{obj}}\,R_{\text{obj}}\,y_{j}+t_{\text{obj}}
$$

**含义**: 将对象点云从局部坐标系对齐到重建的 4D 场景

**符号说明**:
- $y_{j}$: 对象点云中的第 $j$ 个点（局部坐标）
- $y^{\prime}_{j}$: 变换后的点（场景坐标）
- $R_{\text{obj}} \in SO(3)$: 用户控制的旋转矩阵
- $t_{\text{obj}} \in \mathbb{R}^{3}$: 用户控制的平移向量
- $s_{\text{obj}}$: 用户控制的尺度因子

### 公式2: [[Scene Flow Propagation|场景流传播]]

$$
y^{\prime}_{j,t+1}=y^{\prime}_{j,t}+\mathbf{V}_{t\rightarrow t+1}(y^{\prime}_{j,t})
$$

**含义**: 使用场景流动态传播对象位置，处理物体间的物理交互（如盘子上的苹果随盘子移动）

**符号说明**:
- $y^{\prime}_{j,t}$: 第 $t$ 帧对象点的位置
- $\mathbf{V}_{t\rightarrow t+1}$: 从帧 $t$ 到 $t+1$ 的 3D 场景流
- $y^{\prime}_{j,t+1}$: 传播后的下一帧位置

### 公式3: [[Camera Reprojection|相机重投影]]

$$
\begin{bmatrix}u_{j,t}\\ v_{j,t}\\ 1\end{bmatrix}\sim K\left(R_{t}\,y^{\prime}_{j,t}+t_{t}\right)
$$

**含义**: 将 3D 对象点投影到各帧的 2D 图像平面，考虑相机运动和视差

**符号说明**:
- $u_{j,t}, v_{j,t}$: 投影后的 2D 坏素坐标
- $K$: 相机内参矩阵
- $R_{t}, t_{t}$: 第 $t$ 帧的相机外参（旋转和平移）
- $\sim$: 齐次坐标的等价关系

### 公式4: [[DINO Similarity|DINO 相似度评分]]

$$
s_{k}=\frac{1}{N}\sum_{j=1}^{N}\mathrm{DINO}(\hat{o}_{k},f_{j})
$$

**含义**: 评估 VLM 生成的候选对象图与原视频中对象的一致性

**符号说明**:
- $\hat{o}_{k}$: 第 $k$ 个 VLM 生成的对象图候选
- $f_{j}$: 从原视频采样的第 $j$ 帧
- $N$: 采样帧数
- $\mathrm{DINO}(\cdot, \cdot)$: DINO 特征相似度函数
- $s_{k}$: 第 $k$ 个候选的综合相似度得分

### 公式5: [[Best Object Selection|最佳对象选择]]

$$
o^{*}=\arg\max_{\hat{o}_{k}}s_{k}
$$

**含义**: 选择与原视频最匹配的 VLM 生成的对象图作为训练参考

**符号说明**:
- $o^{*}$: 最终选择的参考对象图
- $\hat{o}_{k}$: 第 $k$ 个候选对象图
- $s_{k}$: 对应的相似度得分

---

## 关键图表

### Figure 1: Teaser / 整体效果展示

![Figure 1](https://myyzzzoooo.github.io/InsertAnywhere/static/images/teaser.png)

**说明**: 展示 InsertAnywhere 在真实场景中的效果，包括复杂遮挡和光照适应。

### Figure 2: Overview / 系统概览

![Figure 2](https://arxiv.org/html/2512.17504v1/x1.png)

**说明**: InsertAnywhere 的两阶段架构。Stage 1 通过 4D 场景重建生成几何一致的 mask 序列；Stage 2 使用扩散模型合成对象和光照变化。

### Figure 3: ROSE++ Dataset Generation Pipeline

![Figure 3](https://arxiv.org/html/2512.17504v1/x2.png)

**说明**: ROSE++ 数据集构建流程。从 ROSE 移除数据集出发，通过 VLM 生成参考对象图，形成 (移除视频, 原始视频, 参考图) triplet。

### Figure 4: Qualitative Comparison

![Figure 4](https://arxiv.org/html/2512.17504v1/x3.png)

**说明**: 与 KLing、Pika-Pro 的定性对比。InsertAnywhere 在对象保真度、遮挡处理、尺度控制上显著优于商业工具。

### Figure 5: Ablation Results

![Figure 5](https://arxiv.org/html/2512.17504v1/x4.png)

**说明**: 依次添加各组件的消融效果：(a) 无 4D mask → (b) 加 4D mask → (c) 加首帧 inpainting → 完整模型。4D mask 解决遮挡，首帧 inpainting 提升保真度。

### Figure 6: ROSE++ Finetuning Ablation

![Figure 6](https://arxiv.org/html/2512.17504v1/x5.png)

**说明**: ROSE++ 微调效果。展示了光照适应（开门场景）和阴影生成的改进。

### Table 1: Quantitative Comparisons with Baseline Methods

| Method | CLIP-I ↑ | DINO-I ↑ | Background Consistency ↑ | Subject Consistency ↑ | Motion Smoothness ↑ | Imaging Quality ↑ | Multi-View Consistency ↑ |
|--------|----------|----------|-------------------------|----------------------|--------------------|------------------|-------------------------|
| Pika-Pro | 0.4940 | 0.3856 | 0.9080 | 0.8720 | 0.9889 | 0.6546 | 0.5123 |
| Kling | 0.6349 | 0.5028 | 0.9335 | 0.9494 | **0.9940** | 0.7069 | 0.5439 |
| **Ours** | **0.8122** | **0.5678** | **0.9429** | **0.9520** | 0.9916 | **0.7101** | **0.5857** |

**说明**: InsertAnywhere 在几乎所有指标上超越商业工具，特别是对象一致性 (CLIP-I/DINO-I) 和多视角一致性。

### Table 2: Multi-View Consistency Evaluation

| Method | Multi-View Consistency ↑ |
|--------|-------------------------|
| Ours (random frame reference) | 0.5295 |
| **Ours (VLM-generated reference)** | **0.5857** |

**说明**: VLM 生成的参考图优于随机帧裁剪，避免 copy-and-paste 瑕疵。

### Table 3: Quantitative Ablation Results

| Config | CLIP-I ↑ | DINO-I ↑ | Background Consistency ↑ | Subject Consistency ↑ | Motion Smoothness ↑ | Imaging Quality ↑ | Multi-View Consistency ↑ |
|--------|----------|----------|-------------------------|----------------------|--------------------|------------------|-------------------------|
| (a) w/o 4D mask | 0.7585 | 0.4190 | 0.9206 | 0.9316 | 0.9910 | 0.6175 | 0.5238 |
| (b) + 4D mask | 0.7532 | 0.3861 | 0.9232 | 0.9380 | 0.9913 | 0.6298 | 0.5308 |
| (c) + first-frame inpainting | 0.7880 | 0.5135 | 0.9175 | 0.9290 | 0.9911 | 0.6318 | 0.5436 |
| **Ours (full)** | **0.8122** | **0.5678** | **0.9429** | **0.9520** | **0.9916** | **0.7101** | **0.5857** |

**说明**: 每个组件都有明确贡献，ROSE++ 微调带来最大提升。

### Table 4: User Study Preference Percentages

| Method | Object Fidelity ↑ | Lighting Consistency ↑ | Occlusion Integrity ↑ | Object-Video Consistency ↑ | Background Preservation ↑ | Overall Naturalness ↑ |
|--------|-------------------|-----------------------|----------------------|---------------------------|-------------------------|----------------------|
| Pika-Pro | 1.82% | 3.64% | 0.00% | 1.82% | 2.73% | 4.55% |
| Kling | 19.09% | 25.45% | 13.33% | 21.82% | 16.36% | 24.55% |
| **Ours** | **79.09%** | **71.82%** | **86.67%** | **76.36%** | **80.91%** | **70.00%** |

**说明**: 用户研究中 InsertAnywhere 在所有 6 个维度都大幅领先，遮挡完整性达到 86.67%。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| ROSE++ | 从 ROSE 扩展 | triplet 格式 (移除/原始/参考) | 训练 |
| VOIBench | 50 视频 × 2 对象 | 室内/室外/自然场景，遮挡测试 | 评估 |

### 实现细节

- **视频生成模型**: [[Wan2.1-VACE-14B]]
- **微调方式**: [[LoRA]], rank=128
- **优化器**: 学习率 $1\times 10^{-4}$
- **训练轮数**: 5000 iterations
- **硬件**: 单张 NVIDIA H200 GPU, 约 40 小时
- **推理**: 50 denoising steps
- **分辨率**: $832\times 480$, 81 帧/片段

### 可视化结果

定性结果展示了：
1. **遮挡处理**: 对象正确地在手/人物后面出现
2. **对象保真度**: 尺抽屉保持 3 层（baseline 生成 4 层），胡椒瓶颜色正确
3. **光照适应**: 开门场景光照变化，阴影生成自然

---

## 批判性思考

### 优点

1. **完整的解决方案**: 从用户控制到最终生成，两阶段设计合理
2. **4D 几何理解**: 利用预训练模型组合，无需专门训练 4D 网络
3. **光照监督**: ROSE++ 通过任务反转提供宝贵的光照监督信号
4. **超越商业工具**: 在多个维度显著优于 KLing 和 Pika-Pro

### 局限性

1. **依赖预训练模型**: 4D 重建质量受限于深度估计/光流模型的准确性
2. **静态对象假设**: 仅处理静态插入对象，不支持动态对象插入
3. **合成数据训练**: ROSE++ 为合成数据，真实场景泛化可能有边界
4. **单 GPU 训练**: 需要 H200 级硬件，训练成本高

### 潜在改进方向

1. **动态对象支持**: 扩展到可运动的插入对象
2. **真实数据增强**: 结合真实场景的 insertion/removal 数据
3. **实时推理**: 探索更高效的推理策略
4. **多对象插入**: 支持同时插入多个对象

### 可复现性评估

- [x] 项目主页公开 (https://myyzzzoooo.github.io/InsertAnywhere/)
- [ ] 代码开源 (论文发表后可能公开)
- [ ] ROSE++ 数据集详情
- [x] 训练细节完整 (LoRA rank, 学习率, 硬件)
- [x] 评估基准 VOIBench 介绍完整

---

## 关联笔记

### 基于
- [[Uni4D]]: 4D 场景重建框架
- [[Wan2.1-VACE]]: 基础视频扩散模型
- [[SEA-RAFT]]: 光流估计
- [[SAM2]]: 视频分割
- [[ROSE Dataset]]: 原始物体移除数据集

### 对比
- [[KLing]]: 商业视频生成工具，对比基线
- [[Pika-Pro]]: 商业视频编辑工具，对比基线
- [[GenProp]]: 研究方法，Region-Aware Loss 但无显式遮挡处理

### 方法相关
- [[4D Scene Reconstruction]]: 核心几何技术
- [[Video Inpainting]]: 基础生成技术
- [[Diffusion Model]]: 视频生成架构
- [[LoRA]]: 高效微调方法

### 数据相关
- [[ROSE++]]: 本文提出的 illumination-aware 数据集
- [[VOIBench]]: 本文提出的评估基准

---

## 速查卡片

> [!summary] InsertAnywhere
> - **核心**: 4D 几何 mask + 光照感知扩散模型
> - **方法**: Uni4D 重建 + LoRA 微调 Wan2.1-VACE
> - **结果**: CLIP-I 0.8122，超越 KLing/Pika-Pro
> - **数据**: ROSE++ triplet 格式，VLM 生成参考图

---

*笔记创建时间: 2026-04-03*