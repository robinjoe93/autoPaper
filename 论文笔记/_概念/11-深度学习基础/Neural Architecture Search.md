---
type: concept
aliases: [神经架构搜索, NAS, 自动架构设计]
---

# Neural Architecture Search

## 定义

自动化搜索最优神经网络架构的技术，旨在减少人工设计成本并发现人类可能忽略的有效架构。

## 数学形式

搜索目标：

$$
\mathcal{A}^* = \arg\max_{\mathcal{A} \in \mathcal{S}} \text{Performance}(\mathcal{A}; \theta)
$$

**符号说明**:
- $\mathcal{A}$: 候选架构
- $\mathcal{S}$: 架构搜索空间
- $\theta$: 架构参数

## 核心要点

1. **搜索空间**: 层类型、连接方式、超参数组合
2. **搜索策略**: 强化学习、进化算法、梯度优化、随机搜索
3. **评估效率**: 权重共享(One-Shot)、早停、代理模型
4. **最新趋势**: LLM驱动的架构生成与进化

## 代表工作

- [[ASI-Evolve]]: 进化搜索发现105个SOTA线性注意力架构
- NASNet: 强化学习驱动的架构搜索
- DARTS: 可微分架构搜索

## 相关概念

- [[Evolutionary Search]]
- [[Linear Attention]]
- [[DeltaNet]]