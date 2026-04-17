---
type: concept
aliases: [Cross-Stage Distillation, On-policy Distillation]
---

# On-policy Cross-Stage Distillation

## 定义

一种知识蒸馏技术，在多阶段 RL 训练后，使用前期阶段的模型作为 teacher，通过 on-policy sampling 恢复被遗忘的能力。

## 数学形式

使用 teacher logits 替代 advantage:

$$
\hat{A}_{i,t} = \text{sg} \log \frac{\pi_{teacher}^{infer}(y_{i,t}|x, y_{i,<t})}{\pi_{train}(y_{i,t}|x, y_{i,<t})}
$$

其中 $\text{sg}$ 为 stop gradient 操作。

## 核心要点

1. **能力恢复**: 解决多阶段 RL 的 catastrophic forgetting
2. **On-policy**: 使用 teacher RL 数据分布采样 prompt
3. **Teacher Ensemble**: 可混合多个阶段 teacher
4. **高效训练**: GRPO group size = 1，batch size = 1024

## 代表工作

- [[GLM-5]]: Final refinement stage 使用此技术
- [[MiniLLM]]: On-policy distillation 的原始提出者

## 相关概念

- [[Knowledge Distillation]]: 传统蒸馏技术
- [[GRPO]]: 配合使用的 RL 算法
- [[Catastrophic Forgetting]]: 多阶段训练的核心问题