---
type: concept
aliases: [DrugBAN模型, 双线性注意力DTI]
---

# DrugBAN

## 定义

一种用于药物-靶点相互作用(DTI)预测的深度学习模型，采用双线性注意力网络建模药物分子与蛋白质序列之间的交互。

## 数学形式

双线性注意力：

$$
A = \text{softmax}(E_d W E_p^T)
$$

交互表示：

$$
I_d = A E_p, \quad I_p = A^T E_d
$$

**符号说明**:
- $E_d, E_p$: 药物和蛋白质的编码表示
- $W$: 可学习交互矩阵
- $A$: 注意力权重矩阵

## 核心要点

1. **双线性交互**: 跨模态特征显式交互
2. **可解释性**: 注意力揭示关键子结构对应
3. **冷启动泛化**: ASI-Evolve发现架构在此场景显著提升
4. **架构改进**: Sinkhorn + Marginalization + Top-k

## 代表工作

- [[ASI-Evolve]]: 以DrugBAN为种子架构进化改进

## 相关概念

- [[Drug-Target Interaction]]
- [[Linear Attention]]
- Cross-Modal Fusion