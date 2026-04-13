---
type: concept
aliases: [Vendi 分数, 多样性评分]
---

# Vendi Score

## 定义

Vendi Score 是一种基于 Shannon 多样性指数的数据多样性评估指标，量化数据实例的变异程度。

## 数学形式

$$
\text{Vendi}(S) = \exp\left(\text{tr}(K \log K)\right)
$$

其中 $K$ 是基于相似度的核矩阵。

## 核心要点

1. **语义多样性**: 使用嵌入相似度（如 Qwen3-Embedding）计算
2. **高值高多样性**: Vendi Score 越高表示数据越多样
3. **应用于 SMT**: 用于验证 Focal Rewriting 的多样性提升

## 代表工作

- Friedman and Dieng (2023): 提出 Vendi Score
- [[SMT]]: 用于测量合成文档多样性

## 计算流程

1. 计算每个实例的嵌入（Qwen3-Embedding-8B）
2. 构建 pairwise cosine similarity 矩阵
3. 计算 Vendi Score

## 相关概念

- [[Focal Rewriting]]
- [[Synthetic Mixed Training]]