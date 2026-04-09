---
type: concept
aliases: [线性注意力, 线性复杂度注意力]
---

# Linear Attention

## 定义

一种将标准Transformer注意力的 $O(N^2)$ 复杂度降为 $O(N)$ 的注意力机制变体，通过分解注意力计算或维护压缩记忆状态实现高效序列建模。

## 数学形式

标准注意力：

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d}}\right) V
$$

线性注意力（核函数形式）：

$$
\text{LinearAttn}(Q, K, V) = \frac{Q \left(\phi(K)^T V\right)}{Q \left(\phi(K)^T \mathbf{1}\right)}
$$

**符号说明**:
- $\phi(\cdot)$: 核函数（如elu+1）
- 利用矩阵乘法结合律：$(QK^T)V = Q(K^TV)$

## 核心要点

1. **复杂度降低**: 从 $O(N^2 d)$ 降为 $O(N d^2)$，适合长序列
2. **实现方式**: 核函数分解、状态空间模型(SSM)、Delta规则
3. **权衡**: 通常牺牲部分建模能力换取效率
4. **代表架构**: [[DeltaNet]], Mamba, RWKV, Gated DeltaNet

## 代表工作

- [[ASI-Evolve]]: 发现105个超越DeltaNet的线性注意力架构
- [[DeltaNet]]: 使用Delta规则的线性注意力，ASI-Evolve的基线
- Mamba: 状态空间模型实现线性复杂度

## 相关概念

- [[DeltaNet]]
- [[Neural Architecture Search]]
- Transformer Efficiency