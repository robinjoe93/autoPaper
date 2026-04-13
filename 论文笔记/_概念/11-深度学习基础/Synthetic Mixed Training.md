---
type: concept
aliases: [SMT, 合成混合训练]
---

# Synthetic Mixed Training

## 定义

合成混合训练（SMT）是一种结合合成 QA 数据和合成文档数据的 LLM 训练策略，利用两种数据类型的互补训练信号实现知识学习。

## 数学形式

扩展遵循 log-linear 关系：

$$
\text{Accuracy} \propto \log(\text{Tokens})
$$

## 核心要点

1. **互补性**: QA 教授行为知识（如何使用），文档教授事实知识（存储什么）
2. **混合比例**: QA : Documents = 1 : 1 为最佳
3. **扩展性**: 实现 log-linear 扩展至 700M tokens
4. **突破 RAG**: 在 QuaLITY 上超越 RAG 4.4%

## 代表工作

- [[SMT]]: Han et al. (2026) 提出 SMT 并验证其有效性

## 实现流程

1. 使用 70B 模型生成合成 QA 和 AR 文档
2. 按 1:1 比例混合，加 10% FineWeb replay
3. 对目标 8B 模型进行 continued pretraining

## 相关概念

- [[RAG]]
- [[Focal Rewriting]]
- [[Active Reading]]
- [[QuaLITY]]