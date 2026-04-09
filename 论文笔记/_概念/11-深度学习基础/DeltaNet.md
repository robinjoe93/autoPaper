---
type: concept
aliases: [DeltaNet架构, Delta规则线性注意力]
---

# DeltaNet

## 定义

一种使用Delta规则实现线性复杂度注意力的序列模型，通过维护压缩记忆状态实现高效的长序列建模。

## 数学形式

Delta规则更新：

$$
h_t = h_{t-1} - \alpha_t h_{t-1} + \beta_t v_t
$$

线性注意力实现：

$$
y_t = q_t^T h_t
$$

**符号说明**:
- $h_t$: 时间步 $t$ 的记忆状态
- $\alpha_t, \beta_t$: 可学习的门控参数
- $q_t, v_t$: 查询和值向量

## 核心要点

1. **Delta规则**: 类似增量更新，"遗忘旧信息+写入新信息"
2. **线性复杂度**: $O(N)$ 而非 $O(N^2)$
3. **chunk-wise**: 分块并行计算，兼顾效率与质量
4. **性能基准**: ASI-Evolve架构设计任务的基线

## 代表工作

- [[ASI-Evolve]]: 发现105个超越DeltaNet的架构
- Gated DeltaNet: 引入门控机制改进

## 相关概念

- [[Linear Attention]]
- [[Neural Architecture Search]]
- Mamba (State Space Model)