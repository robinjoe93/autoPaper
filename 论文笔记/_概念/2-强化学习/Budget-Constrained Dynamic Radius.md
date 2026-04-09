---
type: concept
aliases: [预算约束动态半径, BCDR]
---

# Budget-Constrained Dynamic Radius

## 定义

ASI-Evolve发现的RL算法创新（Algorithm B），通过预算约束数学保证总策略更新幅度在预定义范围内，稳定噪声数据上的训练。

## 数学形式

百分位优势归一化：

$$
\hat{A} = \frac{r - c}{s}
$$

全局更新预算约束：

$$
\exp(c) \times |A| \leq z_{cap}
$$

动态信任半径：

$$
\rho_i \propto \frac{1}{|A_i|}
$$

**符号说明**:
- $c$: 中位数，$s$: IQR（百分位归一化）
- $z_{cap}$: 全局更新预算上限
- $\rho_i$: token $i$ 的信任更新半径

## 核心要点

1. **百分位归一化**: 对异常值鲁棒
2. **数学保证**: 预算约束确保总更新有界
3. **动态半径**: 高优势token用小半径（保守）
4. **稳定性**: 有效处理噪声数据

## 代表工作

- [[ASI-Evolve]]: 在RL算法设计任务中自主发现

## 相关概念

- [[GRPO]]
- [[Pairwise Asymmetric Optimization]]
- Policy Gradient Methods