---
type: concept
aliases: [LLM Serving, LLM 服务, 大模型服务]
---

# LLM Serving

## 定义

LLM Serving 指将大型语言模型部署为服务，处理用户请求并返回生成结果的过程，核心挑战是在保证延迟的前提下最大化吞吐量。

## 核心要点

1. **批处理**: 通过批量处理请求提高吞吐量
2. **内存瓶颈**: KV cache 内存管理是关键瓶颈
3. **调度策略**: 迭代级调度、抢占式调度
4. **并行策略**: Tensor Parallelism、Pipeline Parallelism

## 代表工作

- [[vLLM]]: 高吞吐量分布式 LLM 服务引擎
- [[FasterTransformer]]: NVIDIA 推理优化框架
- [[Orca]]: 迭代级调度系统

## 相关概念

- [[KV Cache]]
- [[PagedAttention]]
- [[Inference Optimization]]
- [[Transformer]]