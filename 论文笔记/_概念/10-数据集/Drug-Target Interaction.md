---
type: concept
aliases: [药物-靶点相互作用预测, DTI预测, 蛋白质-药物结合]
---

# Drug-Target Interaction

## 定义

预测药物分子与蛋白质靶点之间相互作用强度的任务，是AI驱动药物发现的核心问题。

## 数学形式

预测目标：

$$
P(y | d, p) = f_\theta(\text{Enc}_d(d), \text{Enc}_p(p))
$$

**符号说明**:
- $d$: 药物分子表示（SMILES、分子图）
- $p$: 蛋白质序列或结构
- $\text{Enc}_d, \text{Enc}_p$: 各模态编码器
- $y$: 结合强度（binary或连续值）

## 核心要点

1. **双模态编码**: 药物（分子图/SMILES）+ 蛋白质（序列/结构）
2. **交互建模**: Cross-attention、双线性融合、图匹配
3. **冷启动挑战**: 预测新药物/新靶点的泛化能力
4. **评估指标**: AUROC, AUPRC, F1, MCC

## 代表工作

- [[ASI-Evolve]]: 发现架构在冷启动场景+6.94 AUROC
- DrugBAN: 双线性注意力网络，ASI-Evolve的基线
- TransformerCPI, PSICHIC: 其他DTI方法

## 相关概念

- [[DrugBAN]]
- Sinkhorn Attention
- AI for Drug Discovery