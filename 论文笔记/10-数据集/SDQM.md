---
title: "SDQM: Synthetic Data Quality Metric for Object Detection Dataset Evaluation"
method_name: "SDQM"
authors: [Ayush Zenith, Arnold Zumbrun, Neel Raut, Jing Lin]
year: 2025
venue: arXiv
tags: [synthetic-data, object-detection, data-quality-metric, dataset-evaluation, domain-gap]
zotero_collection: 待整理
image_source: online
arxiv_html: https://arxiv.org/html/2510.06596v1
created: 2026-04-01
---

# 论文笔记：SDQM: Synthetic Data Quality Metric for Object Detection Dataset Evaluation

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Air Force Research Laboratory (AFRL) |
| 日期 | October 2025 |
| 项目主页 | — |
| 链接 | [arXiv](https://arxiv.org/abs/2510.06596) / [Code](https://github.com/ayushzenith/SDQM) |

---

## 一句话总结

> 提出SDQM——一种无需完整训练即可评估合成数据质量的指标，与YOLOv11的mAP50分数强相关(r=0.87)，显著超越现有评估方法。

---

## 核心贡献

1. **探索现有数据质量指标**: 系统研究多种生成式AI评估指标（[[Alpha-Precision]]、[[Beta-Recall]]、[[Mauve]]、[[FID]]等）对目标检测模型性能预测的效果
2. **提出SDQM综合指标**: 结合像素空间、空间分布、频率空间、特征空间四个维度的域差距评估，使用[[Random Forest]]回归整合多个子指标
3. **验证指标有效性**: 在RarePlanes、DIMO、WASABI三个数据集上验证，SDQM与mAP50的相关性(r=0.87)远超现有方法

---

## 问题背景

### 要解决的问题

评估合成数据对目标检测任务的有效性，避免耗时的训练-验证循环。现有方法需要完整训练模型才能判断数据质量，成本高昂。

### 现有方法的局限

- **[[Inception Score|IS]]**: 倾向于评估类似Inception训练数据的图像，不考虑真实样本统计
- **[[FID]]**: 仅评估分布距离，不直接关联下游任务性能
- **[[SSIM]]**: 内容不变，不适合比较真实与合成图像集合
- **单一指标局限**: 现有指标各关注单一维度（质量、多样性、保真度），无法全面评估数据有用性

### 本文的动机

作者认为需要一种**综合指标**，直接关联下游模型性能，能：
1. 预测合成数据训练后的模型效果
2. 提供可操作的改进建议
3. 减少资源受限场景下的迭代训练需求

---

## 方法详解

### 模型架构

SDQM 采用 **多子指标融合** 架构：
- **输入**: 真实数据集 $\mathcal{X}$ + 合成数据集 $\mathcal{X}'$
- **特征提取**: [[GroundingDINO]]-tiny encoder（最佳表现）
- **子指标计算**: 7个核心子指标
- **融合方法**: [[Random Forest]]回归
- **输出**: SDQM分数（与mAP50正相关）

### 核心子指标

#### 1. [[V-Information|V-Information]]

**设计动机**: 扩展V-Usable Information概念，量化数据集对特定模型族的信息量

**具体实现**:
- **条件熵**: 在合成数据上微调YOLOv11n（冻结backbone）E个epoch后计算
- **预测熵**: 用预训练YOLOv11n验证真实数据
- **相关性最高**: 与mAP50相关性最强（>0.62）

#### 2. [[Beta-Recall]]

**含义**: 测量合成数据覆盖真实数据分布的程度

**高值含义**: 合成数据捕获真实数据的多模态，避免[[Mode Collapse]]

#### 3. [[Dataset Separability]]

**设计动机**: 评估嵌入空间中真实与合成数据的可分性

**具体实现**:
- 使用[[AutoKeras]]选择神经网络架构
- 训练分类器区分真实/合成嵌入
- 记录验证准确率和参数数量

#### 4. [[Pixel Intensity Match]]

**设计动机**: 确保像素强度分布与真实数据相似

**具体实现**:
- 对RGB三通道分别计算像素值分布
- 使用[[Anderson-Darling Test|A-D统计量]]比较分布

#### 5. [[Bounding Box Match]]

**设计动机**: 比较标注框的几何特性

**具体实现**:
- 使用[[Energy Distance]]比较宽高比、对角线长度、面积分布

#### 6. [[Spatial Distribution Difference]]

**设计动机**: 比较目标空间分布

**具体实现**:
- 创建空间分布热力图矩阵
- 计算分布差异分数

#### 7. [[Clusterability]]

**设计动机**: 评估真实与合成嵌入的聚类混合程度

**公式**: 使用Log Cluster Metric

---

## 关键公式

### 公式1: [[Clusterability|Log Cluster Metric]]

$$
L_C(R,S) = \log\left(\frac{1}{k}\sum_{i=1}^{k}\left[\frac{n_i^R}{n_i} - \frac{n^R}{n}\right]^2\right)
$$

**含义**: 量化聚类中真实与合成数据的比例差异

**符号说明**:
- $k$: 聚类数量
- $n$: 数据集总大小
- $n^R$: 真实图像总数
- $n_i$: 聚类 $i$ 中的数据点数
- $n_i^R$: 聚类 $i$ 中的真实数据点数

### 公式2: [[V-Information|V-信息量]]

$$
I_V(X \rightarrow Y) = H_V(Y) - H_V(Y|X)
$$

**含义**: 量化数据集 $X$ 对预测 $Y$ 提供的信息量

**符号说明**:
- $H_V(Y)$: 预测熵
- $H_V(Y|X)$: 条件熵
- $V$: 模型族（本文使用YOLOv11）

### 公式3: 条件熵定义

$$
H_{\mathcal{V}}(Y \mid X) = \inf_{f \in \mathcal{V}} \mathbb{E}\left[-\log_2 f[X](Y)\right]
$$

**含义**: 在模型族 $\mathcal{V}$ 中找到最优预测器后的期望负对数概率

### 公式4: 预测熵定义

$$
H_{\mathcal{V}}(Y) = \inf_{f \in \mathcal{V}} \mathbb{E}\left[-\log_2 f[\emptyset](Y)\right]
$$

**含义**: 无输入数据时的预测难度

---

## 关键图表

### Figure 1: 各子指标与mAP50的相关性

![Figure 1](https://arxiv.org/html/2510.06596v1/figures/pearson_correlation_matrix.png)

**说明**: 展示所有子指标与mAP50的Pearson相关系数。[[V-Information]]相关性最高，所有指标 $\geq 0.62$。

### Figure 2: WASABI数据集目标分布

![Figure 2](https://arxiv.org/html/2510.06596v1/figures/wasabi_count.png)

**说明**: WASABI数据集中小、中、大目标的数量分布。

### Figure 3: SDQM预测vs实际mAP50

![Figure 3](https://arxiv.org/html/2510.06596v1/figures/scatter_plot_random_forest.png)

**说明**: 使用Random Forest系数计算的SDQM值与YOLOv11n mAP50的散点图，相关性 $r=0.87$。

### Table 1: Algorithm 1参数说明

| 参数 | 说明 |
|------|------|
| $\mathcal{X}$ | 真实数据集图像 |
| $\mathcal{X}'$ | 合成数据集图像 |
| $k_l, k_u$ | 子集大小上下界 |
| $g$ | 代数数量 |
| $n$ | 优化值数量 |
| $p$ | 种群大小 |
| $P_m$ | 变异概率 |
| $P_c$ | 交叉概率 |

### Table 2: 各指标与mAP50相关性对比

| Metric | Pearson Correlation |
|--------|---------------------|
| [[FID]] | 0.4477 |
| [[Inception Score|IS]] | 0.5865 |
| [[Alpha-Precision]] | -0.3482 |
| [[Mauve]] | 0.5651 |
| **SDQM** | **0.8719** |

**说明**: SDQM相关性显著高于所有现有指标。

### Table 3: 特征提取模型评估

| Model | Cos Sim Sum | Mean Cos Sim | Euc. Dist. Sum | Mean Euc. Dist. |
|-------|-------------|--------------|----------------|-----------------|
| **GroundingDino-tiny** | **7707.24** | **0.9898** | **760.09** | **0.0976** |
| CLIP-ViT-B/32 | 6949.15 | 0.8924 | 40393.93 | 5.1874 |
| DinoV2-small | 6566.42 | 0.8433 | 129133.36 | 16.5832 |

**说明**: [[GroundingDINO]]-tiny在特征提取任务中表现最佳。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| RarePlanes | 130真实场景+15合成场景 | 航空图像，飞机检测 | 训练/验证 |
| DIMO | 553,800合成图像/42,600场景 | 工业金属物体，Unity生成 | 特征提取评估 |
| WASABI | 100,000真实/20,000合成 | 俯视车辆检测 | 训练/验证 |

### 实现细节

- **特征提取器**: GroundingDINO-tiny encoder
- **目标检测模型**: YOLOv11n
- **回归方法**: Random Forest（最佳），Linear/Ridge/XGBoost次之
- **训练轮数**: 10 epochs（评估用）
- **数据选择**: 使用[[Evolutionary Algorithm]]选择不同相似度的子集对

### 实验流程

1. 使用Algorithm 1（进化算法）选择不同相似度的数据子集对
2. 对每个子集训练YOLOv11n并记录mAP50
3. 计算各子指标值
4. 用回归分析建立SDQM公式
5. K折交叉验证评估相关性稳定性

---

## 批判性思考

### 优点

1. **强相关性验证**: r=0.87远超现有方法，实用价值明确
2. **可操作改进建议**: 通过分析各子指标，可针对性改进数据质量
3. **无需完整训练**: 显著降低评估成本
4. **多维度覆盖**: 像素、空间、频率、特征四维域差距评估

### 局限性

1. **仅限目标检测**: 扩展到其他任务（分割、分类）需要额外验证
2. **依赖预训练模型**: V-Information需要YOLOv11预训练权重
3. **合成数据类型**: 主要针对物理仿真合成数据，生成式AI合成数据效果未验证
4. **数据集依赖**: 在三个特定数据集验证，泛化性需更多实验

### 潜在改进方向

1. 扩展到实例分割、图像分类任务
2. 研究生成式模型（扩散、GAN）产生的合成数据评估
3. 自动化子指标权重调整机制
4. 与主动学习结合优化数据生成

### 可复现性评估

- [x] 代码开源: https://github.com/ayushzenith/SDQM
- [ ] 预训练模型: 未明确提供
- [x] 训练细节完整: Algorithm 1详细描述
- [x] 数据集可获取: RarePlanes/DIMO公开，WASABI需申请

---

## 关联笔记

### 基于

- [[V-Information]]: 扩展V-Usable Information概念
- [[Mauve]]: 嵌入分布相似度评估
- [[Alpha-Precision]] / [[Beta-Recall]]: 生成质量评估

### 对比

- [[FID]]: SDQM相关性(0.87)远超FID(0.45)
- [[Inception Score|IS]]: SDQM更关注下游任务性能

### 方法相关

- [[Object Detection]]: 核心应用任务
- [[YOLO]]: 使用YOLOv11n验证
- [[Domain Gap]]: 评估的核心问题
- [[Synthetic Data]]: 评估对象

### 数据相关

- [[RarePlanes]]: 航空图像数据集
- [[DIMO]]: 工业物体数据集
- [[WASABI]]: 俯视车辆数据集

---

## 速查卡片

> [!summary] SDQM: Synthetic Data Quality Metric
> - **核心**: 无需训练即可预测合成数据对目标检测的效果
> - **方法**: 多子指标融合 + Random Forest回归
> - **结果**: 与mAP50相关性r=0.87，远超FID(0.45)
> - **代码**: https://github.com/ayushzenith/SDQM

---

*笔记创建时间: 2026-04-01*