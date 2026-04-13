---
type: concept
aliases: [Reinforcement Learning from Verifiable Rewards, 可验证奖励强化学习]
---

# RLVR

## 定义

一种基于可验证奖励的强化学习方法，通过将生成的答案与 Ground Truth 进行精确匹配来提供 outcome-level 奖励信号。

## 核心要点

1. **Outcome-level 奖励**: 仅对最终答案进行正确性判断，不提供步骤级反馈
2. **稀疏奖励**: 奖励信号稀疏，训练稳定性较差
3. **依赖 Ground Truth**: 需要可验证的标准答案

## 数学形式

奖励计算：
$$
r_{RLVR}(s) = \mathbb{1}[\text{final answer matches ground truth}]
$$

$$
\hat{A}_{RLVR}^{(i)} = \frac{r_{RLVR}^{(i)} - \mu_G}{\sigma_G}
$$

## 代表工作

- [[SPARK]]: 与 RLVR 对比，PRM-CoT 达到 47.4% > RLVR 43.9%
- DeepSeek-R1: 使用 RLVR 进行推理增强
- Open-Reasoner-Zero: 开源 RLVR 实现

## 相关概念

- [[Process Reward Model|PRM]]: 提供更密集的步骤级反馈
- [[GRPO]]: RLVR 常用的策略优化算法