---
type: concept
aliases: [Memory Management, 内存管理]
---

# Memory Management

## 定义

内存管理指系统对内存资源的分配、回收、优化策略，在 LLM serving 中主要关注 KV cache 的高效管理。

## 核心要点

1. **分配策略**: 预分配 vs 按需分配
2. **碎片处理**: 内部碎片 vs 外部碎片
3. **共享机制**: 引用计数、Copy-on-Write
4. **调度策略**: 抢占、换出换入

## 代表工作

- [[vLLM]]: Block 级内存管理，近零浪费
- [[FasterTransformer]]: 预分配策略

## 相关概念

- [[KV Cache]]
- [[PagedAttention]]
- [[Virtual Memory]]