---
type: concept
aliases: [GRPO, 组相对策略优化, DeepSeek-R1]
---

# GRPO

## 定义

Group Relative Policy Optimization (GRPO)，一种用于大语言模型训练的强化学习算法，通过组内相对比较计算优势值，避免额外价值网络的训练成本。

## 数学形式

组相对优势估计：

$$
A_i = \frac{R_i - \bar{R}_g}{\sigma_g}
$$

**符号说明**:
- $A_i$: 样本 $i$ 的优势值
- $R_i$: 样本 $i$ 的奖励
- $\bar{R}_g$: 组内平均奖励
- $\sigma_g$: 组内奖励标准差

## 核心要点

1. **组内归一化**: 同组样本相互比较，稳定训练
2. **无价值网络**: 简化架构，降低训练成本
3. **KL约束**: 控制策略偏离参考模型的程度
4. **应用场景**: 数学推理、代码生成等LLM后训练

## 代表工作

- DeepSeek-R1: 使用GRPO实现推理能力涌现
- [[ASI-Evolve]]: RL算法设计任务的基线，被进化算法超越+12.5分(AMC32)

## 相关概念

- [[Pairwise Asymmetric Optimization]]
- [[Budget-Constrained Dynamic Radius]]
- Reinforcement Learning from Human Feedback (RLHF)