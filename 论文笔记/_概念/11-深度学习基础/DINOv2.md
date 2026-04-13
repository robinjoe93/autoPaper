---
type: concept
aliases: [DINOv2, DINO v2]
category: 深度学习基础
---

# DINOv2

## 定义

Meta 在 2023 年发布的 DINO 升级版本，更强大的自监督视觉 Transformer。相比 DINO v1，训练数据更大（142M 图像）、架构更强、特征更鲁棒。

## 核心改进

1. **数据规模**: 142M 高质量 curated 图像数据集
2. **架构**: 多种尺寸（ViT-S/g/B/g14）
3. **训练**: 自蒸馏 + iBOT（masked image modeling）
4. **特性**: 密集特征更适合下游任务

## 与 DINO v1 的区别

| 特性 | DINO v1 | DINOv2 |
|------|---------|--------|
| 数据量 | ~1M | 142M |
| 密集特征 | 较弱 | 强 |
| 预训练任务 | 自蒸馏 | 自蒸馏 + iBOT |
| 下游任务 | 分类、检索 | 分类、分割、对应、深度 |

## 在机器人控制中的应用

- **ORCA**: DINOv2 作为 visual encoder，提取 dense features 用于 visual prompts
- 提供帧级细粒度视觉信息，捕获机械臂、物体位置等

## 数学形式

训练损失结合自蒸馏和 iBOT：

$$
\mathcal{L} = \mathcal{L}_{distill} + \lambda \mathcal{L}_{iBOT}
$$

## 代表工作

- [[ORCA]]: 使用 DINOv2 dense features 作为 visual prompts
- DINOv2: Learning Robust Visual Features without Supervision (Oquab et al., 2023)

## 相关概念

- [[DINO]]
- [[ViT]]
- [[Self-supervised Learning]]
- [[Visual Prompt]]