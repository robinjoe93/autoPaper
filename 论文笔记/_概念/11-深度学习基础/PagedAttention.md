---
type: concept
aliases: [PagedAttention, 分页注意力, Page-based Attention]
---

# PagedAttention

## 定义

PagedAttention 是受操作系统虚拟内存分页技术启发的注意力算法，将 KV cache 分块存储在非连续内存空间，通过 block table 映射逻辑序列到物理内存块。

## 数学形式

$$
\text{PagedAttention}(Q_i, K_{blocks}, V_{blocks}, table) = \sum_{b} \sum_{j \in tokens(b)} \frac{\exp(Q_i \cdot K_j^b)}{\sum_{k} \exp(Q_i \cdot K_k)} \cdot V_j^b
$$

## 核心要点

1. **分块存储**: KV cache 分割为固定大小的 block（如 16 tokens）
2. **非连续内存**: 物理块可分散存储，消除碎片
3. **按需分配**: 无需预分配最大长度，近零内存浪费
4. **共享支持**: Block 级引用计数，支持 beam search 共享

## 代表工作

- [[vLLM]]: 提出 PagedAttention 并构建完整系统

## 相关概念

- [[KV Cache]]
- [[Virtual Memory]]
- [[LLM Serving]]
- [[Beam Search]]