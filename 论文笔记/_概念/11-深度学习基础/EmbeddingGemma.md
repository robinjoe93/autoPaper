---
type: concept
aliases: [Gemma Embedding]
---

# EmbeddingGemma

## 定义

Google 的轻量级文本嵌入模型，基于 Gemma 架构，用于生成文本表示向量。

## 数学形式

$$
e = \text{mean\_pooling}(\text{Gemma}_{\text{embed}}(\text{text}))
$$

对 token 嵌入做 mean pooling 得到单个文本表示。

## 核心要点

1. 轻量级嵌入模型
2. 支持多种嵌入任务
3. P2P 用于编码文本指令（冻结）

## 代表工作

- Vera et al. (2025): EmbeddingGemma 论文
- [[P2P]]: 使用 EmbeddingGemma 编码文本

## 相关概念

- [[Vision Language Model]]
- [[P2P]]