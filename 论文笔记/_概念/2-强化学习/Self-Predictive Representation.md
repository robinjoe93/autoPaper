---
type: concept
aliases: [SPR, 自预测表征, Self-Predictive State]
---

# Self-Predictive Representation

## 定义

自预测表征是一种隐状态学习方法，通过约束隐状态能够预测自身未来演化来塑造表征，使其满足近似 Markov 性质。

## 数学形式

自预测损失：
$$
\mathcal{L}_{\text{trans}} = -\sum_{i} \log P(z_{i+1} \mid z_i, a_i)
$$

其中 $z_i = \psi(h_i)$ 是历史编码，目标是使 $z_{i+1}$ 仅通过 $(z_i, a_i)$ 预测，不依赖完整历史。

## 核心要点

1. **Markov 约束**: 隐状态转移应仅依赖当前隐状态和动作
2. **历史压缩**: 不需要完整历史即可预测未来隐状态
3. **辅助正则化**: 通常与奖励预测损失联合使用
4. **表征稳定性**: 减少隐状态的方差，提高学习稳定性

## 与其他方法的关系

- 类似于 [[BYOL-Explore]] 的自预测探索思想
- 与 [[Approximate Information State|AIS]] 的自预测条件一致
- 区别于仅优化预测准确率的方法（如 NAS）

## 代表工作

- [[ASTRA]]: 使用自预测损失进行抽象模拟器修正
- BYOL-Explore: 探究式探索中的自预测表征
- Data-Efficient RL with SPR: 数据高效的 RL 表征学习

## 相关概念

- [[Approximate Information State]]: 理论基础
- [[Latent Dynamics Model]]: 自预测的具体实现
- [[Reward Prediction]]: 联合使用的辅助任务