---
type: concept
aliases: [Muon, Muon Optimizer, Matrix Orthogonalization]
---

# Muon Optimizer

## 定义

一种优化器，通过矩阵正交化技术改善梯度更新，特别适用于 MoE 和 MLA 模型的训练稳定性。

## 数学形式

矩阵正交化（Muon Split 版本）:

对每个 attention head 独立应用:

$$
W_{UQ}^{(h)}, W_{UK}^{(h)}, W_{UV}^{(h)} \leftarrow \text{Orthogonalize}(W^{(h)})
$$

而非整矩阵正交化。

## 核心要点

1. **矩阵正交化**: 使用零除法进行梯度正交化
2. **Muon Split**: 对不同 head 独立应用，允许不同更新 scale
3. **注意力稳定性**: 避免 attention logits 激增
4. **MLA 适配**: 解决 MLA 训练问题，匹配 GQA 性能

## 代表工作

- [[GLM-5]]: Muon + Muon Split 用于 MLA 训练
- [[Muon]]: 原始优化器提出者

## 相关概念

- [[Multi-latent Attention]]: Muon 的主要应用场景
- [[Grouped Query Attention]]: 性能对比基准
- [[Gradient Orthogonalization]]: Muon 的核心技术