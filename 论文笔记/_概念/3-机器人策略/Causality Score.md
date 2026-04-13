---
type: concept
aliases: [因果性评分]
---

# Causality Score

## 定义

衡量策略模型对视觉输入因果依赖程度的指标，通过扰动图像序列后测量输出分布的 KL 散度来计算。

## 数学形式

$$
\text{score}(f) = \sum_{b=1}^{B} \sum_{c=1}^{C} \mathrm{KL}\big(f(o_{b,c}, a_{b,c}) || f(\tilde{o}_{b,c}, a_{b,c})\big)
$$

其中 $o$ 是原始图像，$\tilde{o}$ 是扰动图像（替换为同游戏不同场景），$a$ 是动作序列（不变）。

## 核心要点

1. 高分表示模型更依赖视觉输入而非动作历史
2. 扰动图像而非动作，避免破坏动作序列的因果结构
3. 替换为同游戏不同场景，保持语义合理性

## 代表工作

- [[P2P]]: 首次系统分析 BC 扩展对因果性的影响

## 相关概念

- [[Causal Reasoning]]
- [[KL Divergence]]
- [[Behavior Cloning]]