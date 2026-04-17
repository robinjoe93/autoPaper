---
type: concept
aliases: [DSA, DeepSeek-Sparse-Attention, DeepSeek 稀疏注意力]
---

# DeepSeek Sparse Attention

## 定义

一种动态稀疏注意力机制，通过 Lightning Indexer 检索最相关的 KV entries，只在这些子集上计算注意力，实现长上下文的无损计算效率提升。

## 数学形式

$$
\text{DSA}(Q, K, V) = \text{SparseAttn}(Q, K_{\text{top-k}}, V_{\text{top-k}})
$$

其中 $K_{\text{top-k}}, V_{\text{top-k}}$ 由 Indexer 检索：

$$
K_{\text{top-k}} = \text{Indexer}(Q, K) = \text{TopK}(\text{Score}(Q, K))
$$

## 核心要点

1. **动态稀疏**: 基于内容而非固定模式选择 token
2. **无损性能**: 长上下文 benchmark 性能与 dense attention 相当
3. **训练策略**: Dense warm-up + Sparse adaptation
4. **计算效率**: 128K 长上下文计算成本降低 1.5-2x

## 代表工作

- [[GLM-5]]: 采用 DSA 实现 200K 长上下文
- [[DeepSeek-V3]]: DSA 的原始提出者

## 相关概念

- [[Multi-latent Attention]]: 与 DSA 结合使用的 KV cache 压缩技术
- [[Sliding Window Attention]]: 固定模式的稀疏注意力
- [[Mixture of Experts]]: 类似的动态选择思想