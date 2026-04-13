---
type: concept
aliases: [Binary Cross Entropy Loss, 二元交叉熵损失]
---

# BCE Loss

## 定义
Binary Cross Entropy 损失，用于二分类任务，衡量两个 binary distribution 之间的差异。

## 数学形式

$$
\mathcal{L}_{\text{BCE}} = -\sum_{i} \left[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \right]
$$

## 在 3D Completion 中的应用

Occupancy grid 是 binary（占用/不占用），BCE 用于对齐观测区域：

$$
\mathcal{L}_{\text{align}} = \text{BCE}(\boldsymbol{S}_{0|t} \odot \boldsymbol{M}, \boldsymbol{S}_{p} \odot \boldsymbol{M})
$$

## 核心要点
1. **Binary 适用**: 适合 occupancy grid 表示
2. **梯度友好**: 可用于 latent space 的梯度优化

## 代表工作
- [[LaS-Comp]]: IAS 使用 BCE 作为 alignment loss

## 相关概念
- [[IAS]]
- [[Alignment Loss]]
- [[Occupancy Grid]]
- [[MSE Loss]]