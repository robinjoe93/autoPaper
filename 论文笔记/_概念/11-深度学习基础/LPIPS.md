---
title: "LPIPS"
created: 2026-04-20
tags: [evaluation-metric, perceptual-quality, image-similarity]
category: 11-深度学习基础
---

# LPIPS

## 定义

[[LPIPS]] (Learned Perceptual Image Patch Similarity) 是一种基于深度网络特征学习的感知图像相似度度量，相比传统像素级指标更能反映人类视觉感知。

## 核心思想

- **特征级比较**: 比较深度网络提取的特征而非原始像素
- **感知相关**: 特征差异更符合人类视觉感知判断
- **学习权重**: 通过人类判断数据学习特征权重

## 数学表示

$$
\text{LPIPS}(x, y) = \sum_l w_l \cdot d_l(x, y)
$$

其中：
- $l$: 深度网络的不同层
- $w_l$: 学习的层权重
- $d_l$: 层特征的距离度量（如L2距离）

## 与传统指标对比

| 指标 | 基础 | 优势 | 局限 |
|------|------|------|------|
| PSNR | 像素级MSE | 计算简单 | 不反映感知质量 |
| SSIM | 结构相似性 | 考虑结构 | 仍有限 |
| LPIPS | 深度特征 | 符合感知 | 需预训练网络 |

## 在Lyra中的应用

[[Lyra]] 使用LPIPS作为关键损失函数：
- $\mathcal{L}_{lpips}$: 感知损失项
- 消融实验表明去除LPIPS损失后：
  - PSNR: 24.77 → 23.74
  - SSIM: 0.837 → 0.766
  - LPIPS: 0.224 → 0.370（显著恶化）

## 重要性

在生成任务中，LPIPS特别重要：
- 传统像素指标可能误导优化方向
- 感知质量对用户体验更关键
- 在Lyra消融中LPIPS影响显著

## 实现

通常使用预训练网络提取特征：
- AlexNet
- VGG
- SqueezeNet

## 相关概念

- [[PSNR]]: 传统像素级指标
- [[SSIM]]: 结构相似性指标
- [[Lyra]]: 使用LPIPS损失的应用实例

## 参考文献

Zhang et al., "The Unreasonable Effectiveness of Deep Features as a Perceptual Metric" (CVPR 2018)