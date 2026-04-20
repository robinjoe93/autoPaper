---
type: concept
aliases: [部分可观测, POMDP, 部分可观测马尔可夫决策过程]
---

# Partial Observability

## 定义

部分可观测性是指智能体无法完全观测环境状态，只能获得部分信息或带有噪声的观测，导致 Markov 性质失效的情况。

## 数学形式

部分可观测马尔可夫决策过程 (POMDP)：
$$
\mathcal{M} = (\mathcal{S}, \mathcal{A}, P, r, \mathcal{O}, \Omega, \gamma)
$$

其中：
- $\mathcal{O}$: 观测空间
- $\Omega: \mathcal{S} \times \mathcal{A} \rightarrow \Delta(\mathcal{O})$: 观测函数

最优策略需基于历史：
$$
\pi^*(a|h) = \pi^*(a|s_1, a_1, o_2, ..., o_i)
$$

## 核心要点

1. **Markov 失效**: 观测不能充分表征真实状态
2. **历史依赖**: 最优决策需要历史信息
3. **信念状态**: 维护状态分布的后验估计
4. **信息状态**: 历史的充分统计量

## 在抽象 Sim2Real 中的应用

状态抽象 $\phi: \mathcal{S}^t \rightarrow \mathcal{S}^s$ 诱导部分可观测性：

- 多个真实状态 $s^t$ 可能映射到相同抽象状态 $s^s$
- 这些真实状态可能有不同的动力学
- 抽象状态 Markov 性质失效

## 解决方案

1. **历史策略**: 使用 RNN/GRU 处理历史序列
2. **信念状态**: 维护状态分布估计
3. **近似信息状态**: 学习历史的压缩表示

## 代表工作

- [[ASTRA]]: 抽象诱导部分可观测性的解决方案
- Deep Recurrent Q-Learning: POMDP 的深度学习方法

## 相关概念

- [[State Abstraction]]: 诱导部分可观测性的原因
- [[Approximate Information State]]: 解决方案
- [[GRU]]: 处理历史的网络结构
- [[POMDP]]: 理论框架