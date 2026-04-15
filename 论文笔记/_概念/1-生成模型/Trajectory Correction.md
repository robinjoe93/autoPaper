---
type: concept
aliases: [轨迹修正, bias correction]
---

# Trajectory Correction

## 定义

在扩散模型的 post-training 中，通过构造模型自身产生的偏离轨迹状态（off-trajectory states），并监督模型将这些状态修正回归正确干净目标的方法。

## 数学形式

偏离状态构造：
$$
\hat{z}_{\sigma_{t_1}} = z_{\sigma_{t_0}} + (\sigma_{t_1} - \sigma_{t_0}) v_{cfg}
$$

修正目标：
$$
v_{corr} = \frac{z'_\sigma - z_0}{\sigma}
$$

修正损失：
$$
\mathcal{L}_{corr} = \|v_\theta(z'_\sigma, c, \sigma) - v_{corr}\|^2
$$

## 核心要点

1. **On-policy**: 偏离状态由当前模型产生，随模型参数进化
2. **稠密监督**: 每个 timestep 都有明确修正目标，无信用分配问题
3. **无奖励模型**: 修正目标从训练样本解析推导
4. **单步 rollout**: 使用单步 ODE 保持偏差有界，确保 z_0 作为正确锚点
5. **共享噪声**: 重加噪使用相同 z_1，保持辅助状态在原始传输路径邻域

## 代表工作

- [[SOAR]]: Self-Correction for Optimal Alignment and Refinement，轨迹修正方法的核心实现

## 相关概念

- [[Exposure Bias]]
- [[Flow Matching]]
- [[Rectified Flow]]
- [[Post-training]]