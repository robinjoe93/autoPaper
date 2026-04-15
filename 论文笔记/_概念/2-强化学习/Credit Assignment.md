---
type: concept
aliases: [信用分配, credit assignment problem]
---

# Credit Assignment

## 定义

强化学习中确定哪些动作或状态对最终奖励贡献最大的问题。在序列决策中，终端奖励信号稀疏，需要将奖励反向分配到数十或数百个中间步骤。

## 数学形式

梯度估计：
$$
\nabla_\theta J(\theta) = \mathbb{E}_{\pi_\theta} \left[ \nabla_\theta \log \pi_\theta(a|s) \cdot A(s, a) \right]
$$

优势函数估计：
$$
A(s, a) = Q(s, a) - V(s) = \sum_{t'=t}^T \gamma^{t'-t} r_{t'} - V(s)
$$

## 核心要点

1. **稀疏奖励**: 终端奖励仅在完整 rollout 后可用
2. **长序列困难**: 扩散模型有数十个去噪步骤，信用分配困难
3. **时序偏差**: 早期动作的影响难以准确估计
4. **解决方案**: 使用稠密中间奖励、价值函数估计、或避免信用分配问题

## 代表工作

- [[Flow-GRPO]]: 使用 group-relative advantage 缓解信用分配
- [[SOAR]]: 通过轨迹修正完全避免信用分配问题

## 相关概念

- [[Reinforcement Learning]]
- [[Advantage Function]]
- [[Value Function]]
- [[Reward Hacking]]