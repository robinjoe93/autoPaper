---
title: "SoftMimicGen: A Data Generation System for Scalable Robot Learning in Deformable Object Manipulation"
method_name: "SoftMimicGen"
authors: [Masoud Moghani, Mahdi Azizian, Animesh Garg, Yuke Zhu, Sean Huver, Ajay Mandlekar]
year: 2026
venue: ICRA 2026
tags: [deformable-object-manipulation, synthetic-data-generation, imitation-learning, sim-to-real, robot-learning]
zotero_collection: AI研究方法
image_source: online
arxiv_html: https://arxiv.org/html/2603.25725v1
created: 2026-04-03
---

# 论文笔记：SoftMimicGen: A Data Generation System for Scalable Robot Learning in Deformable Object Manipulation

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | NVIDIA, University of Toronto, Georgia Institute of Technology |
| 日期 | March 2026 |
| 项目主页 | [softmimicgen.github.io](https://softmimicgen.github.io) |
| 对比基线 | [[MimicGen]] |
| 链接 | [arXiv](https://arxiv.org/abs/2603.25725) |

---

## 一句话总结

> SoftMimicGen 是首个针对可变形物体操作的自动化数据生成系统，通过[[Non-Rigid Registration|非刚性配准]]技术从少量人类演示生成大规模数据集，支持四种机器人平台和十种高精度操作任务。

---

## 核心贡献

1. **数据生成管道**: 提出首个针对可变形物体操作的自动化数据生成系统，从 1-10 个人类演示生成大规模数据集
2. **仿真环境套件**: 创建高保真软体仿真环境，涵盖毛绒玩具、绳子、纸巾、毛巾等多种可变形物体
3. **多平台支持**: 支持四种机器人平台（单臂 Franka、双臂 YAM、人形 GR1、手术机器人 dVRK）的十种操作任务

---

## 问题背景

### 要解决的问题

大规模机器人数据集难以收集和扩展，需要大量人力、时间和成本。现有合成数据生成方法仅限于刚体任务，而可变形物体操作占据了现实世界操作的重要部分，却缺乏有效的数据生成方案。

### 现有方法的局限

- [[MimicGen]] 等方法依赖静态物体参考帧进行轨迹生成，但可变形物体不存在固定参考帧
- 刚体任务仿真容易，但软体仿真计算复杂度高
- 可变形物体具有连续、高维的配置空间，数据生成困难

### 本文的动机

作者认为通过[[Non-Rigid Registration|非刚性配准]]技术可以解决可变形物体缺乏固定参考帧的问题，结合现代软体仿真器的高效性能，可以实现可变形物体操作的大规模数据生成。

---

## 方法详解

### 模型架构

SoftMimicGen 采用 **数据生成管道** 架构：

- **输入**: 少量人类遥操作演示 (1-10 个)
- **核心模块**: [[Non-Rigid Registration|非刚性配准]] 用于场景匹配，[[Soft Body Simulation|软体仿真器]] 用于物理模拟
- **输出**: 大规模合成演示数据集
- **下游**: 训练 [[Imitation Learning|模仿学习]] 策略

### 核心模块

#### 模块1: 非刚性配准

**设计动机**: 可变形物体不存在静态参考帧，需要通过点集配准来建立源演示与新场景之间的对应关系

**具体实现**:
- 将源物体配置 $O = \{\mathbf{u_i}\}_{i=1}^{N_O}$ 与新场景配置 $O' = \{\mathbf{v_j}\}_{j=1}^{N_{O'}}$ 进行配准
- 使用优化方法找到变形函数 $\mathbf{f}$，最小化配准代价
- 利用仿真器提供的 ground-truth 节点信息实现精确配准

#### 模块2: 轨迹适应

**设计动机**: 将源演示轨迹变换到新场景配置

**具体实现**:
- 使用变形函数 $\mathbf{f}$ 变换末端执行器位置
- 通过 Jacobian 矩阵 $\mathbf{J_f}$ 变换姿态
- 分段处理每个子任务的轨迹

#### 模块3: 源演示选择

**设计动机**: 智能选择与新场景最相似的源演示，提高数据生成质量

**具体实现**:
- 观察当前子任务的对象配置
- 与所有源演示段的开端配置进行配准比较
- 选择配准代价最低的源演示段

---

## 关键公式

### 公式1: [[Non-Rigid Registration|变形变换]]

$$
p_{t}\rightarrow\mathbf{f}(p_{t}),\quad R_{t}\rightarrow\mathrm{orth}(\mathbf{J_{f}}(p_{t})R_{t})
$$

**含义**: 将源演示中的末端执行器位置 $p_t$ 和姿态 $R_t$ 通过变形函数 $\mathbf{f}$ 及其 Jacobian 变换到新场景

**符号说明**:
- $p_t$: 源演示中的末端执行器位置
- $R_t$: 源演示中的末端执行器姿态
- $\mathbf{f}$: 变形函数
- $\mathbf{J_f}$: 变形函数的 Jacobian 矩阵
- $\mathrm{orth}(\cdot)$: 正交化操作，确保旋转矩阵的有效性

### 公式2: 源演示段选择

$$
\tau \in \mathcal{D}_{\text{src}}
$$

**含义**: 从源数据集 $\mathcal{D}_{\text{src}}$ 中选择演示段 $\tau$ 用于生成新演示

**符号说明**:
- $\tau$: 演示轨迹段
- $\mathcal{D}_{\text{src}}$: 源演示数据集

### 公式3: 轨迹分段表示

$$
\tau_{i}=(T^{C_{0}}_{W},T^{C_{1}}_{W},...,T^{C_{K}}_{W})
$$

**含义**: 演示段 $\tau_i$ 由一系列末端执行器控制姿态组成

**符号说明**:
- $\tau_i$: 第 $i$ 个演示段
- $T^{C_k}_{W}$: 世界坐标系 $W$ 到控制坐标系 $C_k$ 的变换矩阵
- $K$: 轨迹中的控制点数量

---

## 关键图表

### Figure 1: System Pipeline / 系统概览

![Figure 1](https://arxiv.org/html/2603.25725v1/2603.25725v1/Fig/Fig/Fig2.png)

**说明**: SoftMimicGen 的整体架构。输入少量人类演示，通过[[Non-Rigid Registration|非刚性配准]]和轨迹适应，自动生成大规模数据集用于训练机器人策略。

### Figure 2: Simulation Tasks / 仿真任务

![Figure 2](https://arxiv.org/html/2603.25725v1/2603.25725v1/Fig/Fig/Fig3.png)

**说明**: SoftMimicGen 支持的 10 种仿真任务，涵盖四种机器人平台：(a-b) GR1 人形机器人、(c-f) Franka 单臂、(g-h) dVRK 手术机器人、(i-j) YAM 双臂。任务包括高精度穿线、动态鞭打、折叠、抓取放置等。

### Figure 3: Real-World Deployment / 真实世界部署

![Figure 3](https://arxiv.org/html/2603.25725v1/2603.25725v1/Fig/Fig/Fig4.png)

**说明**: 真实世界评估的三种任务：毛巾折叠、绳子操作、袋子装载。展示了从仿真到真实的迁移能力。

### Figure 4: Registration & Transfer (项目主页)

![Registration](https://softmimicgen.github.io/static/images/registration.png)

**说明**: 非刚性配准过程示意，展示如何将源物体配置映射到新场景配置。

![Transfer](https://softmimicgen.github.io/static/images/transfer.png)

**说明**: 轨迹转移过程示意，展示如何将源演示轨迹变换到新场景执行。

### Table 1: Simulation Performance Comparison

| Task | Source Demo BC-RNN-GMM | Generated Demo BC-RNN-GMM |
|------|-------------------------|---------------------------|
| Humanoid - Teddy | - | - |
| Humanoid - Towel | - | - |
| Franka - Rope | - | - |
| Franka - Jenga | - | - |
| Franka - Towel | - | - |
| Franka - Rigid Cube | - | - |
| Surgical - Tissue | - | - |
| Surgical - Threading | - | - |
| YAM - Towel | - | - |
| YAM - Bag Loading | - | - |

**说明**: 比较源演示数据集与生成数据集训练的策略性能，验证生成数据的有效性。

### Table 2: Dataset Size Comparison

| Task | 50 Demos | 250 Demos | 500 Demos | 750 Demos |
|------|----------|-----------|-----------|-----------|
| Humanoid - Teddy | 24.0 | 26.0 | 44.0 | 44.0 |
| Humanoid - Towel | 61.3 | 70.7 | 64.0 | 64.0 |
| Franka - Rope | 82.7 | 100.0 | 98.7 | 93.3 |
| Franka - Jenga | 53.3 | 77.3 | 93.3 | 84.0 |
| Franka - Towel | 81.3 | 76.0 | 74.7 | 84.0 |
| Franka - Rigid Cube | 42.0 | 68.0 | 82.7 | 84.0 |
| Surgical - Tissue | 69.3 | 56.0 | 84.0 | 82.7 |
| Surgical - Threading | 56.0 | 84.0 | 98.7 | 96.0 |
| YAM - Towel | 8.0 | 12.0 | 9.3 | 17.3 |
| YAM - Bag Loading | 6.7 | 20.0 | 17.3 | 17.3 |

**说明**: 数据集规模对策略性能的影响。成功率随数据量增加而提升，突显大规模数据生成的重要性。

### Table 3: Real-World Deployment Results

| Task | Real (30 Demos) | Zero-shot Sim (1000 Demos) | Sim-Real Co-Train (1000+30 Demos) |
|------|-----------------|----------------------------|-----------------------------------|
| Franka - Towel | 76.6 | 70.0 | 76.6 |
| Franka - Rope | 46.7 | 33.3 | 76.6 |
| YAM - Bag Loading | 33.3 | 63.3 | 93.3 |

**说明**: 真实世界评估结果。Sim-Real Co-Training（仿真数据+少量真实数据）效果最佳，验证了 SoftMimicGen 生成数据的有效性。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Source Demos | 1-10 个 | 人类遥操作 | 数据生成源 |
| Generated Demos | 500-750 个 | 自动生成 | 策略训练 |
| Real Demos | 30 个 | 真实世界遥操作 | Co-training |

### 实现细节

- **策略架构**: BC-RNN-GMM (Behavior Cloning with RNN and GMM)
- **仿真器**: 高保真软体物理仿真（实时或更快）
- **训练方式**: 纯仿真数据 / 真实数据 / Sim-Real Co-training
- **下游策略**: [[Point Bridge]] 用于 Sim-to-Real 迁移

### 任务类型

1. **高精度任务**: Surgical Threading（穿线）、Jenga（叠叠乐）
2. **动态任务**: Rope whipping（绳子鞭打）
3. **折叠任务**: Towel folding（毛巾折叠）
4. **抓取任务**: Teddy pick-and-place（毛绒玩具抓取）
5. **装载任务**: Bag Loading（袋子装载）

### 关键发现

1. 数据量增加显著提升策略性能
2. Sim-Real Co-training 效果优于纯真实数据训练
3. Zero-shot Sim-to-Real 可实现有效迁移
4. YAM 双臂任务难度最高（成功率较低）

---

## 批判性思考

### 优点

1. 首个针对可变形物体操作的数据生成系统，填补了重要空白
2. 支持多种机器人平台和任务类型，泛化能力强
3. 利用仿真器 ground-truth 信息实现精确配准，避免噪声干扰
4. 真实世界验证充分，Sim-Real Co-training 效果显著

### 局限性

1. 假设固定的子任务序列，难以处理非结构化任务
2. 软体仿真仍有一定计算开销
3. YAM 双臂任务成功率较低，复杂任务仍有挑战
4. 需要高保真仿真环境，依赖特定物理引擎

### 潜在改进方向

1. 扩展支持灵活的任务结构和条件转移
2. 结合更多样化的可变形物体类型
3. 探索在线配准用于推理阶段
4. 与视觉-语言-动作模型（如 $\pi_0$）结合

### 可复现性评估

- [x] 项目主页公开
- [ ] 代码开源（待发布）
- [ ] 仿真环境细节描述
- [x] 训练设置详细说明
- [x] 数据集规模和来源说明

---

## 关联笔记

### 基于

- [[MimicGen]]: 刚体操作的数据生成系统，SoftMimicGen 的核心基础
- [[Non-Rigid Registration]]: 非刚性配准技术，解决可变形物体参考帧问题

### 对比

- [[GenAug]]: 生成式数据增强方法
- [[Diffusion Policy]]: 另一种模仿学习策略架构

### 方法相关

- [[Imitation Learning]]: 核心学习范式
- [[Sim-to-Real Transfer]]: 仿真到真实迁移
- [[Soft Body Simulation]]: 软体物理仿真
- [[Point Bridge]]: 用于真实世界部署的表示方法

### 任务相关

- [[Deformable Object Manipulation]]: 可变形物体操作

---

## 速查卡片

> [!summary] SoftMimicGen
> - **核心**: 非刚性配准驱动的可变形物体数据生成
> - **方法**: 从 1-10 个演示生成大规模数据集
> - **结果**: Sim-Real Co-training 达 93.3% 成功率（Bag Loading）
> - **项目**: [softmimicgen.github.io](https://softmimicgen.github.io)

---

*笔记创建时间: 2026-04-03*