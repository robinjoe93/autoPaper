---
type: concept
aliases: [KV Cache, KV cache, Key-Value Cache]
---

# KV Cache

## 定义

KV Cache 是 Transformer 模型在自回归生成过程中缓存的历史 token 的 Key 和 Value 向量，用于避免重复计算，加速后续 token 的生成。

## 数学形式

$$
K_i = W_K \cdot h_i, \quad V_i = W_V \cdot h_i
$$

$$
\text{Attention}(Q_i, K, V) = \sum_{j=1}^{i} \text{softmax}(Q_i \cdot K_j^T) \cdot V_j
$$

## 核心要点

1. **动态增长**: 每个 token 生成后追加一对新的 K/V 向量
2. **内存占用**: 约占 LLM serving GPU 内存的 30%
3. **管理挑战**: 大小不可预知、生命周期不确定、易产生碎片

## 代表工作

- [[vLLM]]: PagedAttention 分页管理 KV cache
- [[FasterTransformer]]: NVIDIA 的高效 KV cache 实现
- [[Orca]]: 迭代级调度系统

## 相关概念

- [[Transformer]]
- [[PagedAttention]]
- [[LLM Serving]]
- [[Attention Score]]