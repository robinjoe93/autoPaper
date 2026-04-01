---
type: concept
aliases: [Attention Score, 注意力分数, Attention Weight]
---

# Attention Score

## 定义

注意力分数表示 Query token 对各 Key token 的关注程度，通过 softmax 归一化后作为加权权重。

## 数学形式

$$
\text{score}_{ij} = \frac{Q_i \cdot K_j^T}{\sqrt{d_k}}
$$

$$
\text{Attention}_{ij} = \frac{\exp(\text{score}_{ij})}{\sum_{k} \exp(\text{score}_{ik})}
$$

## 核心要点

1. **点积计算**: Query 与 Key 的相似度度量
2. **缩放因子**: 除以 $\sqrt{d_k}$ 防止梯度消失
3. **Softmax 归一化**: 转换为概率分布

## 代表工作

- [[Transformer]]: 基础架构
- [[PagedAttention]]: 分页计算注意力分数

## 相关概念

- [[KV Cache]]
- [[Transformer]]