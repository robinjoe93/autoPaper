---
type: concept
aliases: [检索增强生成, Retrieval-Augmented Generation]
---

# RAG

## 定义

检索增强生成（RAG）是一种将外部知识检索与语言模型生成相结合的技术，用于增强模型在知识密集型任务上的表现。

## 核心要点

1. **两阶段流程**: 先检索相关文档，再基于检索内容生成回答
2. **突破参数限制**: 无需将所有知识存储在模型参数中
3. **知识可更新**: 外部知识库可独立更新，无需重新训练模型

## 代表工作

- [[SMT]]: 合成数据训练突破 RAG 性能天花板
- Lewis et al. (2020): RAG 原始论文

## 技术细节

典型实现：
- **Retriever**: 嵌入模型（如 Qwen3-Embedding）检索 top-k 文档块
- **Reranker**: 重排序模型筛选最相关 chunks
- **Generator**: LLM 基于检索内容生成回答

## 相关概念

- [[Synthetic Mixed Training]]
- [[QuaLITY]]
- [[LongHealth]]