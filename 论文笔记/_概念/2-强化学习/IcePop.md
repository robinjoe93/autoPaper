---
type: concept
aliases: [IcePop, IcePop technique]
---

# IcePop

## 定义

一种用于 MoE 模型 RL 训练的技术，通过区分 training policy 和 inference policy 来解决 training-inference mismatch 问题。

## 数学形式

Training-inference mismatch ratio:

$$
\rho_{i,t} = \frac{\pi_{\theta_{old}}^{train}(y_{i,t}|x, y_{i,<t})}{\pi_{\theta_{old}}^{infer}(y_{i,t}|x, y_{i,<t})}
$$

Pop operator 截断极端 mismatch:

$$
\text{pop}(\rho, 1/\beta, \beta) = \begin{cases} \rho, & \text{if } 1/\beta \leq \rho \leq \beta \\ 0, & \text{otherwise} \end{cases}
$$

## 核心要点

1. **Policy 区分**: 显式区分 $\pi_{train}$ 和 $\pi_{infer}$
2. **Mismatch 校正**: 使用 importance sampling ratio 调整梯度
3. **极端值截断**: 移除 mismatch 过大的样本
4. **移除 KL**: 加速 RL improvement（GLM-5 的改进）

## 代表工作

- [[GLM-5]]: GRPO + IcePop 作为 RL backbone
- [[DeepSeek-V3]]: IcePop 的原始提出者

## 相关概念

- [[GRPO]]: 与 IcePop 结合的 RL 算法
- [[Router Replay]]: MoE 的类似问题解决方案
- [[PPO]]: 传统 RL 算法