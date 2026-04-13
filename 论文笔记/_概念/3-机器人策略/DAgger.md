---
type: concept
aliases: [Dataset Aggregation]
---

# DAgger

## 定义

Dataset Aggregation，通过在线收集专家纠正数据解决分布偏差的模仿学习方法。

## 数学形式

迭代流程：
$$
\mathcal{D}_{t+1} = \mathcal{D}_t \cup \{(s, a_{\text{expert}}(s))\}_{s \sim \pi_t}
$$

在策略 $\pi_t$ 遇到的状态下收集专家动作。

## 核心要点

1. 解决 BC 的分布偏差问题
2. 策略探索新状态，专家提供纠正
3. 需要在线专家交互

## 代表工作

- Ross et al. (2011): DAgger 论文
- [[P2P]]: 使用纠正数据（<1% 数据）

## 相关概念

- [[Behavior Cloning]]
- [[Imitation Learning]]
- [[Causal Reasoning]]