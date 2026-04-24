---
title: "Image Generators are Generalist Vision Learners"
method_name: "VisionBanana"
authors: [Valentin Gabeur, Shangbang Long, Songyou Peng, Paul Voigtlaender, Shuyang Sun, Yanan Bao, Karen Truong, Zhicheng Wang, Wenlei Zhou, Jonathan T. Barron, Kyle Genova, Nithish Kannen, Sherry Ben, Yandong Li, Mandy Guo, Suhas Yogin, Yiming Gu, Huizhong Chen, Oliver Wang, Saining Xie, Howard Zhou, Kaiming He, Thomas Funkhouser, Jean-Baptiste Alayrac, Radu Soricut]
year: 2026
venue: arXiv
tags: [generative-vision-pretraining, foundation-model, image-generation, semantic-segmentation, depth-estimation, surface-normal]
zotero_collection: _待整理
image_source: online
arxiv_html: https://arxiv.org/html/2604.20329
created: 2026-04-24
---

# 论文笔记：Image Generators are Generalist Vision Learners

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Google DeepMind |
| 日期 | April 2026 |
| 项目主页 | http://vision-banana.github.io |
| 链接 | [arXiv](https://arxiv.org/abs/2604.20329) |

---

## 一句话总结

> 证明图像生成预训练是通用视觉学习的有效范式，Vision Banana 通过将视觉任务输出参数化为 RGB 图像，在分割和深度估计等任务上达到零样本 SOTA。

---

## 核心贡献

1. **范式验证**: 首次系统证明图像生成预训练（类似 LLM 预训练）可以学习强大且通用的视觉表示
2. **统一接口**: 将感知任务重新定义为图像生成，通过 RGB 图像作为统一输出格式
3. **SOTA 性能**: 在 2D（语义/实例/指代分割）和 3D（深度/表面法线）任务上超越或匹敌领域专家模型

---

## 问题背景

### 要解决的问题

图像生成模型是否具备强大的视觉理解能力？类似于 LLM 从生成式预训练中涌现语言理解和推理能力，图像生成器是否也能涌现视觉理解能力？

### 现有方法的局限

- 传统的视觉理解模型通常针对特定任务设计，缺乏通用性
- 现有的基础视觉模型（如 [[SAM]]、[[DINO]]）主要基于判别式预训练，而非生成式
- 之前对生成式模型的理解能力研究较为零散，缺乏系统性验证

### 本文的动机

作者观察到：
1. 创建视觉内容的能力暗示着理解该内容的能力
2. [[LLM]] 的生成式预训练成功展示了涌现能力
3. 图像生成模型在训练过程中必须理解图像的语义和结构才能生成高质量输出

---

## 方法详解

### 模型架构

Vision Banana 基于 **Nano Banana Pro (NBP)** 进行指令微调：

- **基座模型**: [[Nano Banana Pro]] — 一个强大的图像生成模型
- **核心思想**: 将视觉任务输出参数化为 RGB 图像
- **训练方式**: 在原始生成数据 + 少量视觉任务数据上进行混合指令微调
- **任务统一**: 感知任务 → 图像生成任务

### 任务参数化方式

#### 语义分割

将分割 mask 编码为彩色图像，通过自然语言指令指定颜色映射：

```
"The cats are represented by (255, 0, 0).
The background is represented by (0, 0, 0)."
```

#### 深度估计

将深度值编码为 RGB 图像，使用 colormap（如 rainbow）可视化：

```
"Predict the metric depth of this scene as an image.
Visualized in the rainbow colormap."
```

#### 表面法线估计

直接生成表面法线可视化图像：

```
"Predict the surface normal of this scene."
```

### 训练细节

- **数据混合**: 原始生成数据 + 少量视觉任务数据
- **轻量微调**: 仅需少量任务数据即可达到 SOTA
- **能力保持**: 微调后仍保持基座模型的图像生成能力

---

## 关键图表

### Figure 1: Overview / 系统概览

![Figure 1: Teaser](http://vision-banana.github.io/files/images/teaser_img_v6.png)

**说明**: Vision Banana 从生成式预训练到视觉理解的范式转换。图像生成预训练作为视觉任务的通用基础。

### Figure 2: 语义分割示例

![Semantic Segmentation](http://vision-banana.github.io/files/images/semseg/img2.jpg)

![Semantic Segmentation Result](http://vision-banana.github.io/files/images/semseg/img2-prompt2.png)

**说明**: 通过自然语言指令指定颜色映射，Vision Banana 生成语义分割结果。

### Figure 3: 实例分割示例

![Instance Segmentation](http://vision-banana.github.io/files/images/insseg/insseg2.jpg)

![Instance Segmentation Result](http://vision-banana.github.io/files/images/insseg/insseg2_1.png)

**说明**: 每个实例用不同颜色表示，通过指令指定分割目标。

### Figure 4: 指代分割示例

![Referring Segmentation](http://vision-banana.github.io/files/images/refseg/refseg5_in.png)

![Referring Segmentation Result](http://vision-banana.github.io/files/images/refseg/refseg5_out.png)

**说明**: 通过自然语言描述指代特定对象进行分割。

### Figure 5: 深度估计示例

![Depth Estimation](http://vision-banana.github.io/files/images/depth/depth_fallingwater_0.jpg)

![Depth Estimation Result](http://vision-banana.github.io/files/images/depth/depth_fallingwater_1.png)

**说明**: 单目度量深度估计，使用 rainbow colormap 可视化。

### Figure 6: 表面法线估计示例

![Surface Normal](http://vision-banana.github.io/files/images/normal/normal_jazz_0.jpg)

![Surface Normal Result](http://vision-banana.github.io/files/images/normal/normal_jazz_1.png)

**说明**: 表面法线估计，准确捕捉几何结构。

---

## 实验

### 2D 理解任务结果

#### Cityscapes 语义分割（Zero-shot）

| Method | mIoU |
|--------|------|
| APE-D | 0.442 |
| OpenSeeD | 0.478 |
| X-Decoder | 0.520 |
| SAM 3 | 0.652 |
| **Vision Banana** | **0.699** |

**说明**: Vision Banana 在零样本语义分割上超越 SAM 3。

#### SA-Co/Gold 实例分割（Zero-shot）

| Method | pmF1 |
|--------|------|
| APE-D | 0.369 |
| OWLv2 | 0.420 |
| Gemini 2.5 | 0.461 |
| DINO-X | 0.552 |
| **Vision Banana** | **0.540** |

#### RefCOCOg 指代分割

| Method | cIoU |
|--------|------|
| HybridGL | 0.513 |
| Kang + LLaVA | 0.677 |
| SAM 3 + Gemini 2.5 Pro | 0.734 |
| **Vision Banana** | **0.738** |

### 3D 理解任务结果

#### 度量深度估计（6 个 Benchmark 平均）

| Method | δ₁ |
|--------|------|
| Depth Pro | 0.715 |
| MoGe-2 | 0.802 |
| UniK3D | 0.823 |
| **Vision Banana** | **0.882** |

**说明**: Vision Banana 在度量深度估计上显著领先，且不需要相机内参。

#### 表面法线估计（3 个 Benchmark 平均）

| Method | Mean Angular Error (°) |
|--------|------|
| Marigold | 19.606 |
| StableNormal | 17.168 |
| DSINE | 17.017 |
| Lotus-2 | 16.558 |
| **Vision Banana** | **15.549** |

---

## 批判性思考

### 优点

1. **范式创新**: 首次系统性验证生成式预训练作为视觉理解范式
2. **统一接口**: RGB 图像作为统一输出格式，简化多任务学习
3. **零样本 SOTA**: 无需任务特定训练即可达到顶尖性能
4. **能力保持**: 微调后仍保持生成能力

### 局限性

1. 输出格式受限于 RGB 图像分辨率
2. 依赖基座模型 Nano Banana Pro 的质量
3. 对于需要精确数值输出的任务可能需要额外后处理

### 潜在改进方向

1. 探索更多视觉任务（如光流、姿态估计）
2. 提升输出分辨率以获得更精细结果
3. 与 [[VLA]] 结合用于具身智能

### 可复现性评估

- [x] 项目主页公开
- [ ] 代码开源（待发布）
- [ ] 模型权重（待发布）
- [x] 训练方法详细描述

---

## 关联笔记

### 基于
- [[Nano Banana Pro]]: 基座图像生成模型
- [[SAM]]: 对比的分割基线
- [[Depth Anything]]: 对比的深度估计基线

### 方法相关
- [[Generative Pretraining]]: 核心训练范式
- [[Foundation Model]]: 基础模型概念
- [[Instruction Tuning]]: 训练方法

---

## 速查卡片

> [!summary] Vision Banana
> - **核心**: 图像生成预训练 = 通用视觉学习
> - **方法**: 任务输出参数化为 RGB 图像 + 指令微调
> - **结果**: 分割/深度/法线 Zero-shot SOTA
> - **项目**: http://vision-banana.github.io

---

*笔记创建时间: 2026-04-24*