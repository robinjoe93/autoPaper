---
type: concept
aliases: [UCB, Upper Confidence Bound, 置信上限]
---

# UCB Score

## 定义

Upper Confidence Bound算法的核心评分公式，用于平衡探索与利用的选择策略。

## 数学形式

$$
\text{UCB}(a) = \text{score}(a) + c \sqrt{\frac{\log(N+1)}{n(a)+1}}
$$

其中：
- $\text{score}(a)$: 智能体 $a$ 的性能分数
- $N$: 总访问次数
- $n(a)$: 智能体 $a$ 的子代数量
- $c$: 探索权重系数

## 核心要点

1. **平衡探索利用**: 高性能智能体获得更多选择机会
2. **不确定性奖励**: 子代少的智能体获得探索奖励
3. **自动发现**: DGM-H 自动发现了 UCB 策略作为父代选择机制
4. **经典算法**: UCB 是多臂赌博机问题的经典解决方案

## 代表工作

- [[Hyperagents]]: 自动发现 UCB 用于父代选择
- Auer et al. (2002): UCB 原始论文

## 相关概念

- [[Open-endedness]]: UCB 支持的开端探索策略
- [[Meta Agent]]: 使用 UCB 进行父代选择
- [[Parent Selection]]: UCB 应用的选择问题