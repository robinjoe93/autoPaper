---
type: concept
aliases: [上置信界采样, UCB算法, 探索-利用平衡]
---

# UCB1 Sampling

## 定义

基于多臂赌博机(Multi-Armed Bandit)理论的上置信界采样策略，平衡已观察价值与探索潜力，选择最有希望的节点进行下一轮进化。

## 数学形式

UCB1选择公式：

$$
n^* = \arg\max_n \left( \hat{Q}(n) + c \sqrt{\frac{\ln N}{n_{visits}}} \right)
$$

**符号说明**:
- $\hat{Q}(n)$: 节点 $n$ 的估计价值（历史平均得分）
- $c$: 探索常数（ASI-Evolve用 $c=1.414$）
- $N$: 总采样次数
- $n_{visits}$: 节点 $n$ 的访问次数

## 核心要点

1. **价值引导**: 高得分节点有更高选中概率
2. **探索激励**: 低访问节点获得探索bonus
3. **理论保证**: 亚线性遗憾界
4. **适用场景**: 认知库辅助下快速收敛优于多样性策略

## 代表工作

- [[ASI-Evolve]]: UCB1+GPT-5-mini仅17轮达SOTA（vs MAP-Elites 79轮）

## 相关概念

- [[Evolutionary Search]]
- [[MAP-Elites]]
- [[Cognition Base]]