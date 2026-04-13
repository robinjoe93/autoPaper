---
type: concept
aliases: [因果推理]
---

# Causal Reasoning

## 定义

理解变量间因果关系的推理能力，在行为克隆中指模型基于真正因果因素（如视觉输入）而非虚假相关因素（如动作历史）做决策。

## 数学形式

因果正确策略应满足：
$$
p(a|s_{\text{cause}}) \neq p(a|s_{\text{non-cause}})
$$

如玩具问题中 $c = \log p(a=1|s_o) - \log p(a=1|s_b)$，最优因果策略 $c \to \infty$。

## 核心要点

1. 行为克隆常见因果混淆：模型依赖历史动作而非视觉
2. 扩展模型和数据规模可改善因果推理能力
3. 非线性网络比线性网络更容易学习因果策略（SGD）

## 代表工作

- [[P2P]]: 扩展 BC 改善因果推理
- De Haan et al. (2019): 因果混淆问题

## 相关概念

- [[Causality Score]]
- [[Behavior Cloning]]
- [[DAgger]]