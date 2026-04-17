---
type: concept
aliases: [TITO, Token-in-Token-out, Token-Level Consistency]
---

# TITO

## 定义

Token-in-Token-out，一种异步 RL 训练中保证一致性的机制，直接传递 token IDs 和 metadata 而非通过 text round-trip。

## 核心要点

1. **无重 tokenize**: 避免 token boundary mismatch
2. **Action-level correspondence**: 保持采样和优化的一致性
3. **Streaming 支持**: Actor 可立即发送 trajectory fragments
4. **Gateway 设计**: 集中处理 token ID 相关逻辑

## 代表工作

- [[GLM-5]]: Agentic RL 使用 TITO Gateway

## 相关概念

- [[Asynchronous Agent RL]]: TITO 的应用场景
- [[Text-in-Text-out]]: TITO 的对立方案（有问题）
- [[DDIS]]: 配合使用的 importance sampling 技术