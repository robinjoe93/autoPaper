---
type: concept
aliases: [Descendant Growth Score, Stepping Stone Score]
---

# Growth Score

## 定义

衡量智能体作为 stepping stone 价值的指标，优先选择能稳定产生更好后代的智能体用于迁移。

## 数学形式

$$
G_\gamma(i) = \frac{1}{|D(i)|} \sum_{j \in D(i)} (\alpha_j - \alpha_i) \cdot \gamma^{\text{dist}(i,j)}
$$

其中：
- $D(i)$: 智能体 $i$ 的后代集合
- $\alpha_j$: 智能体 $j$ 的评估分数
- $\gamma \in (0, 1]$: 衰减因子
- $\text{dist}(i,j)$: 代际距离

## 核心要点

1. **后代质量**: 重视能产生高质量后代的智能体
2. **代际衰减**: 近代后代权重更高
3. **迁移选择**: 用于选择跨域迁移的智能体
4. **避免局部最优**: 不只看当前分数，关注改进潜力

## 代表工作

- [[Hyperagents]]: 用于迁移实验的智能体选择

## 相关概念

- [[Improvement@k]]: 互补的改进度量
- [[Open-endedness]]: stepping stone 理论基础
- [[Meta Agent]]: 产生后代的元级智能体