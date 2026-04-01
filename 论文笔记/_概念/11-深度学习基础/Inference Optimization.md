---
type: concept
aliases: [Inference Optimization, 推理优化, 模型推理加速]
---

# Inference Optimization

## 定义

Inference Optimization 指通过各种技术手段加速深度学习模型推理过程，降低延迟、提高吞吐量，减少计算和内存开销。

## 核心要点

1. **计算优化**: Kernel fusion、算子优化
2. **内存优化**: KV cache 管理、量化压缩
3. **并行策略**: Tensor parallelism、Pipeline parallelism
4. **调度优化**: 批处理、迭代级调度

## 代表工作

- [[vLLM]]: KV cache 内存优化
- [[FasterTransformer]]: Kernel 级优化
- [[TensorRT]]: NVIDIA 推理引擎

## 相关概念

- [[LLM Serving]]
- [[KV Cache]]
- [[PagedAttention]]