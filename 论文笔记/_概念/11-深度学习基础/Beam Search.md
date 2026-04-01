---
type: concept
aliases: [Beam Search, 束搜索]
---

# Beam Search

## 定义

Beam Search 是一种启发式搜索算法，在每一步保留 top-k 个最优候选序列，平衡搜索质量与计算开销，常用于文本生成任务。

## 数学形式

$$
\text{score}(y_1, ..., y_t) = \sum_{i=1}^{t} \log P(y_i | y_1, ..., y_{i-1})
$$

保留 top-$k$ 个序列继续扩展。

## 核心要点

1. **束宽度 k**: 保留候选数量，k 越大搜索越全面但计算越重
2. **序列共享**: 不同候选可共享前缀 KV cache
3. **内存优化**: 通过 KV cache 共享减少重复计算

## 代表工作

- [[vLLM]]: Beam Search 场景下 KV cache 共享优化效果显著

## 相关概念

- [[KV Cache]]
- [[PagedAttention]]
- [[LLM Serving]]