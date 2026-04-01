---
type: concept
aliases: [Orca]
---

# Orca

## 定义

Orca 是迭代级调度的 LLM serving 系统，允许不同请求在不同迭代层次加入批次，提高 GPU 利用率。

## 核心要点

1. **迭代级调度**: 请求可在任意迭代加入批次
2. **选择性批处理**: 根据请求进度动态调整
3. **KV Cache**: 预分配连续内存，存在碎片问题

## 代表工作

- 作为 [[vLLM]] 的对比基线之一

## 相关概念

- [[LLM Serving]]
- [[KV Cache]]
- [[FasterTransformer]]