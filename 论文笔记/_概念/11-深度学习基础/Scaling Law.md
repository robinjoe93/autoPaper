---
type: concept
aliases: [扩展定律, 缩放定律]
---

# Scaling Law

## 定义

描述模型性能（如损失）与模型规模、数据量、计算量之间幂律关系的经验规律。

## 数学形式

$$
L(D) = L_{\infty} + \left(\frac{D_c}{D}\right)^{\alpha}
$$

其中 $L(D)$ 是损失，$D$ 是数据量，$L_\infty$ 是不可约损失，$D_c$ 和 $\alpha$ 是拟合常数。

## 核心要点

1. 损失随数据量呈幂律下降
2. 更大模型在数据充足时受益更多
3. 可预测给定计算预算下的最优模型/数据配比

## 代表工作

- Kaplan et al. (2020): LLM 扩展定律
- [[P2P]]: 行为克隆的扩展定律分析

## 相关概念

- [[Transformer]]
- [[Foundation Models]]