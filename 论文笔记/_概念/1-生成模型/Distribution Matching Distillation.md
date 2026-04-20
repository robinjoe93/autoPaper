---
title: Distribution Matching Distillation
created: 2026-04-20
tags: [distillation, diffusion, acceleration, generative-model]
---

# Distribution Matching Distillation (DMD)

## 定义

Distribution Matching Distillation (DMD) 是一种用于加速扩散模型推理的蒸馏方法。它扩展了 Variational Score Distillation (VSD) 的思想，通过近似 KL 散度将多步扩散模型蒸馏为少步生成器。

## 核心公式

$$
\nabla\mathcal{L}_{\text{DMD}}=-\mathbb{E}_{t}\left(\int\left(s_{\text{real}}(x_{t},t)-s_{\text{fake}}(x_{t},t)\right)\frac{dx_{t}}{d\theta}dz\right)
$$

其中：
- $s_{\text{real}}(x_{t},t)$: 冻结的真实分数函数
- $s_{\text{fake}}(x_{t},t)$: 可训练的假分数函数
- $x=G_{\theta}(z)$: 学生生成器
- $z\sim\mathcal{N}(0;\mathbf{I})$: 随机高斯噪声
- $t\sim\mathcal{U}(0,1)$: 时间采样

## 训练过程

1. **初始化**: $s_{\text{real}}$、$G_{\theta}$、$s_{\text{fake}}$ 都从同一 VDM 初始化
2. **冻结真实分数**: $s_{\text{real}}$ 保持冻结
3. **训练**: $G_{\theta}$ 和 $s_{\text{fake}}$ 全部可训练
4. **频率**: $s_{\text{fake}}$ 每次生成器更新训练5次
5. **稳定**: 使用随机梯度截断稳定训练

## 关键技术

### 真实分数函数
从预训练教师模型初始化，提供去噪方向的"真实"分布信息。

### 假分数函数
与学生生成器共同训练，学习学生分布的分数。

### KL 散度近似
通过两个分数函数的差异构建近似 KL 散度梯度。

## 在 WorldStereo 2.0 中的应用

[[WorldStereo 2.0]] 使用修改的 DMD 将模型蒸馏为 4-step DiT：

- 支持记忆训练的全蒸馏（不同于 WorldStereo 仅蒸馏相机控制）
- 同时提升相机控制精度和记忆能力
- 略增加训练成本但效果显著

## 优势

1. **快速推理**: 从数百步蒸馏到少数步
2. **质量保持**: 保持生成质量
3. **灵活训练**: 支持完整功能蒸馏

## 与其他蒸馏方法对比

| 方法 | 特点 |
|------|------|
| [[DMD]] | 分布匹配、两个分数函数 |
| [[Consistency Models]] | 一致性约束 |
| [[Progressive Distillation]] | 渐进蒸馏 |

## 应用场景

- [[WorldStereo 2.0]]: 关键帧生成加速
- 图像生成加速
- 视频生成加速

## 相关概念

- [[Diffusion Model]]: 基础扩散模型
- [[Variational Score Distillation]]: DMD 的扩展基础
- [[Consistency Models]]: 替代加速方法

## 参考文献

- Yin et al. (2024): "Improved Distribution Matching Distillation for Fast Image Synthesis"
- HY-World Team (2026): "HY-World 2.0"