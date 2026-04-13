---
type: concept
aliases: [计算量估计, FLOPs 计算]
---

# Compute Estimation

## 定义

计算量估计是评估 LLM 训练所需计算资源的方法，通常以 FLOPs 计量。

## 数学形式

合成训练的总计算量：

$$
C \approx 2MD + 6ND
$$

**含义**: 合成训练的总 FLOPs

**符号说明**:
- $C$: 总 FLOPs
- $M$: 生成器参数量
- $N$: 目标模型参数量
- $D$: 合成 token 数量
- $2MD$: 数据生成计算（forward pass）
- $6ND$: 模型训练计算（forward + backward）

## 核心要点

1. **前向计算**: 2×参数×tokens（forward pass）
2. **反向计算**: 4×参数×tokens（backward pass）
3. **生成成本**: 合成数据生成需额外计算

## 代表工作

- Kaplan et al. (2020): 提出 scaling laws 计算
- [[SMT]]: 用于估计合成训练成本

## 实例

训练 8B 模型于 70B 生成的 700M tokens：
- $C \approx 1.316 \times 10^{20}$ FLOPs
- 约 18.5 H100 hours

## 相关概念

- [[Synthetic Mixed Training]]