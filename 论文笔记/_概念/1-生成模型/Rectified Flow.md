---
type: concept
aliases: [直化流, rectified flow matching]
---

# Rectified Flow

## 定义

一种 flow matching 方法，通过学习直线 velocity field 从噪声分布传输到数据分布，使采样路径为直线从而实现高效 ODE 采样。

## 数学形式

线性插值路径：
$$
z_t = (1 - t) z_0 + t z_1, \quad t \in [0, 1]
$$

条件 velocity：
$$
u(z_t | z_0, z_1) = z_1 - z_0
$$

流匹配目标：
$$
\mathcal{L}_{FM}(\theta) = \mathbb{E}_{z_0, z_1, c, t} \|v_\theta(z_t, c, t) - (z_1 - z_0)\|^2
$$

干净端点估计：
$$
\hat{z}_0(z, \sigma) = z - \sigma \cdot v_\theta(z, c, \sigma)
$$

## 核心要点

1. **直线路径**: 每对 $(z_0, z_1)$ 定义一条从数据到噪声的直线传输路径
2. **常速度**: 条件 velocity 在路径上保持恒定（$z_1 - z_0$）
3. **高效采样**: 直线路径使 ODE 采样步数大幅减少
4. **主流架构**: SD3.5、FLUX 等先进模型采用 rectified flow

## 代表工作

- [[SOAR]]: 基于 rectified flow 实现轨迹修正
- Liu et al. (2023): Flow Straight and Fast，原始 rectified flow 论文
- [[SD3.5]]: Stable Diffusion 3.5 使用 rectified flow

## 相关概念

- [[Flow Matching]]
- [[Diffusion Model]]
- [[ODE Sampling]]
- [[Transport Theory]]