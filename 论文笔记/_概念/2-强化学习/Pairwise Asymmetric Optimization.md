---
type: concept
aliases: [成对不对称优化, PAO]
---

# Pairwise Asymmetric Optimization

## 定义

ASI-Evolve发现的RL算法创新（Algorithm A），通过成对奖励差值计算优势而非组均值，并采用不对称裁剪和梯度dropout稳定训练。

## 数学形式

成对优势估计：

$$
A_i = \frac{1}{n-1} \sum_{j \neq i} \tanh(R_i - R_j)
$$

不对称裁剪：

$$
\epsilon_{down} = \begin{cases} \epsilon_0 & \text{if } A > 0 \\ k\epsilon_0 & \text{if } A < 0 \end{cases}
$$

高影响梯度dropout：

$$
g_i = \begin{cases} 0 & \text{if } p_i \times A_i \in \text{TopK} \\ g_i & \text{otherwise} \end{cases}
$$

**符号说明**:
- $\tanh$: 归一化成对奖励差
- $\epsilon_{down}, \epsilon_{up}$: PPO裁剪窗口
- $p_i \times A_i$: token影响度（概率×优势）

## 核心要点

1. **成对比较**: 避免组均值噪声，更鲁棒
2. **不对称裁剪**: 正负优势用不同裁剪阈值
3. **梯度dropout**: 防止过拟合高影响关键词
4. **性能提升**: AMC32 +12.5分超越[[GRPO]]

## 代表工作

- [[ASI-Evolve]]: 在RL算法设计任务中自主发现

## 相关概念

- [[GRPO]]
- [[Budget-Constrained Dynamic Radius]]
- PPO (Proximal Policy Optimization)