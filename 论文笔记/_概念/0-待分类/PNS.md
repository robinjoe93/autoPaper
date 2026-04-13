---
type: concept
aliases: [Partial-aware Noise Schedule, 部分感知噪声调度]
---

# PNS (Partial-aware Noise Schedule)

## 定义
LaS-Comp 中提出的噪声调度策略，根据 spatial mask 区分观测区域和缺失区域的 stochasticity。

## 数学形式

$$
\boldsymbol{x}^*_{1|t} = \boldsymbol{M} \odot \left(\sqrt{1-t} \cdot \hat{\boldsymbol{x}}_{1|t} + \sqrt{t} \cdot \boldsymbol{\epsilon}_1\right) + (1 - \boldsymbol{M}) \odot \boldsymbol{\epsilon}_2
$$

## 核心要点
1. **观测区域**: 混合预测 latent 和噪声，早迭代允许调整，晚迭代趋于稳定
2. **缺失区域**: 纯噪声，鼓励探索多样性
3. **消除伪影**: 消融实验显示移除 PNS 导致条纹伪影

## 时间依赖系数
- $\sqrt{1-t}$: 预测 latent 权重（随 $t$ 减小，晚迭代权重增大）
- $\sqrt{t}$: 噪声权重（随 $t$ 减小，晚迭代噪声减小）

## 代表工作
- [[LaS-Comp]]: 提出并使用 PNS

## 相关概念
- [[ERS]]
- [[Noise Schedule]]
- [[Diffusion Sampling]]