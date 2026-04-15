---
type: concept
aliases: [曝光偏差, train-inference mismatch]
---

# Exposure Bias

## 定义

序列生成模型（如自回归语言模型或扩散模型）中，训练时使用 ground-truth 状态作为条件，但推理时使用模型自身预测作为条件，导致训练与推理状态分布不匹配的现象。

## 数学形式

训练状态分布（扩散模型）：
$$
p_\sigma^{\text{data}}(z) = \int \delta((1-\sigma)z_0 + \sigma z_1)(z) \, q(z_0) \mathcal{N}(z_1) \, dz_0 dz_1
$$

推理状态分布：
$$
\hat{z}_\sigma = z_1 + \int_\sigma^1 v_\theta(\hat{z}_s, c, s) \, ds
$$

误差累积：
$$
\|\hat{z}_0 - z_0^{ideal}\| \leq \frac{1}{K} \sum_{k=1}^K \|\epsilon(\sigma_k)\|
$$

## 核心要点

1. **自回归模型**: 训练时 teacher forcing，推理时自回归生成，早期 token 错误传播
2. **扩散模型**: 训练时前向过程状态，推理时模型自身 ODE rollout 状态，早期去噪错误累积
3. **分布差异**: $p_\sigma^{\text{data}}$ vs $p_\sigma^\theta$，差距随采样步数增长
4. **解决方法**: Scheduled sampling、输入扰动、轨迹修正（SOAR）

## 代表工作

- [[SOAR]]: 扩散模型轨迹修正，直接解决曝光偏差
- Bengio et al. (2015): 自回归模型的 scheduled sampling
- Ning et al. (2023): 扩散模型输入扰动

## 相关概念

- [[Teacher Forcing]]
- [[Scheduled Sampling]]
- [[Trajectory Correction]]
- [[Autoregressive Model]]