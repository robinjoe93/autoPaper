---
type: concept
aliases: [Flow Matching, 流匹配]
---

# Flow Matching

## 定义
一种生成模型训练方法，通过学习从噪声到数据的 continuous flow path，比 Diffusion 更高效地生成样本。

## 数学形式

Linear flow path: $\boldsymbol{x}_t = t \cdot \boldsymbol{x}_1 + (1 - t) \cdot \boldsymbol{x}_0$

Velocity field: $\boldsymbol{v}_t = \boldsymbol{x}_1 - \boldsymbol{x}_0$

Generator 学习预测 velocity: $\mathcal{G}(\boldsymbol{x}_t, t) \approx \boldsymbol{v}_t$

## 核心要点
1. **ODE-based**: 通过 ODE solver 快速采样
2. **相比 Diffusion**: 无需迭代 denoising，采样更快
3. **应用**: 3D generation（DORA）、图像生成

## 与 LaS-Comp 的关系
LaS-Comp 使用 Flow Matching generator（DORA backbone）：
- $\hat{\boldsymbol{x}}_{0|t} = \boldsymbol{x}_t - t \cdot \mathcal{G}(\boldsymbol{x}_t, t)$（noise-free estimation）
- $\hat{\boldsymbol{x}}_{1|t} = \boldsymbol{x}_t + (1 - t) \cdot \mathcal{G}(\boldsymbol{x}_t, t)$（noisy estimation）

## 代表工作
- [[DORA]]: 3D VAE + Flow Matching
- [[LaS-Comp]]: 使用 Flow Matching generator

## 相关概念
- [[DORA]]
- [[3D Foundation Model]]
- [[Diffusion Model]]
- [[ODE Solver]]