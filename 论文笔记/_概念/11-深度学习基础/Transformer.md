---
type: concept
aliases: [Transformer, Transformer架构]
---

# Transformer

## 定义

Transformer 是基于自注意力机制的序列模型架构，通过并行计算注意力权重实现高效的序列建模，是现代大语言模型的基础架构。

## 数学形式

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(head_1, ..., head_h)W^O
$$

## 核心要点

1. **自注意力**: Token 之间直接计算相互关系
2. **位置编码**: 补偿序列顺序信息
3. **多头注意力**: 多个子空间并行学习不同关系
4. **残差连接**: 缓解深层网络训练问题

## 代表工作

- [[vLLM]]: Transformer 模型的服务优化
- [[GPT]]: 基于 Transformer 的生成式模型

## 相关概念

- [[KV Cache]]
- [[Attention Score]]
- [[Autoregressive Decomposition]]