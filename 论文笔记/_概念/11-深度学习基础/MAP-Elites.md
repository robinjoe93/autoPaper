---
type: concept
aliases: [MAP-Elites算法, 质量-多样性档案, 多维档案进化]
---

# MAP-Elites

## 定义

一种质量-多样性(Quality-Diversity)进化算法，维护按行为特征分维度的档案，在保持解空间多样覆盖的同时优化各维度内的最优解。

## 数学形式

档案更新：

$$
\mathcal{A}[b(p)] = \begin{cases} p & \text{if } f(p) > f(\mathcal{A}[b(p)]) \\ \mathcal{A}[b(p)] & \text{otherwise} \end{cases}
$$

**符号说明**:
- $\mathcal{A}$: 档案网格
- $b(p)$: 将解 $p$ 映射到行为空间的坐标函数
- $f(p)$: fitness函数

## 核心要点

1. **多维行为空间**: 按预设特征维度划分档案单元格
2. **精英保留**: 每个单元格仅保留fitness最高的解
3. **多样性保证**: 被动保持多样性，无需显式多样性压力
4. **探索-平衡**: 防止过早收敛，鼓励广泛覆盖

## 代表工作

- [[ASI-Evolve]]: 作为采样策略选项，配合认知库使用
- QD-RL: 强化学习中的质量-多样性优化

## 相关概念

- [[Evolutionary Search]]
- [[UCB1 Sampling]]
- [[Evolutionary Agent]]