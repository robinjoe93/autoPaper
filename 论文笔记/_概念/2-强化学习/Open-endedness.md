---
type: concept
aliases: [开端式, 开放式探索, Open-ended Exploration]
---

# Open-endedness

## 定义

系统持续发明新的、有趣的、日益复杂的产物，在没有固定目标或预设终点的情况下扩展自身发现边界的能力。

## 核心要点

1. **持续探索**: 不收敛到单一最优解，持续发现新的可能性
2. **多样性积累**: 成功的中间产物作为 stepping stones 保留
3. **避免早熟收敛**: 通过种群/档案机制避免陷入局部最优
4. **无界限改进**: 理论上可无限改进，不受预设上限限制

## 数学形式

档案增长过程：

$$
\mathcal{A}_t = \mathcal{A}_{t-1} \cup \{(a_t, s_t)\}
$$

其中 $a_t$ 是新生成的智能体，$s_t$ 是其评估分数。

## 代表工作

- [[Hyperagents]]: 使用开端探索支持元认知自我修改
- [[Darwin Godel Machine]]: 编码领域的开端式自我改进
- Stanley et al. (2017): 开端式理论基础
- Hughes et al. (2024): 开端式AI综述

## 相关概念

- [[Quality-Diversity Algorithms]]: 开端式探索的算法实现
- [[Stepping Stones]]: 开端式积累的中间产物
- [[Meta Agent]]: 驱动开端式探索的元级智能体