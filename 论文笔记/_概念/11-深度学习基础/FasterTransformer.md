---
type: concept
aliases: [FasterTransformer, FT]
---

# FasterTransformer

## 定义

FasterTransformer 是 NVIDIA 开发的高效 Transformer 推理框架，通过 kernel fusion、量化等技术加速模型推理。

## 核心要点

1. **Kernel Fusion**: 融合多个算子减少内存访问
2. **INT8 量化**: 降低计算和内存开销
3. **KV Cache**: 预分配连续内存存储
4. **Tensor Parallelism**: 多 GPU 并行

## 代表工作

- 作为 [[vLLM]] 的主要对比基线

## 相关概念

- [[LLM Serving]]
- [[KV Cache]]
- [[Inference Optimization]]