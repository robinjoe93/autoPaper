---
type: concept
aliases: [梯度嵌入, Gradient-based Embedding]
---

# Gradient Embedding

## 定义

梯度嵌入是一种将训练数据的梯度信息转换为向量表示的技术，用于分析不同数据类型的训练信号相似度。

## 数学形式

使用 next-token prediction loss 和 Johnson-Lindenstrauss 变换降维：

$$
g_i = \text{JL}(\nabla_\theta \mathcal{L}_i)
$$

## 核心要点

1. **梯度相似度**: 使用 cosine similarity 比较不同数据类型
2. **互补性验证**: QA 与文档梯度相似度低（≤0.26），验证互补
3. **模型**: 使用小模型（如 Qwen3 0.6B）计算梯度

## 代表工作

- Jung et al. (2025): 提出 Prismatic Synthesis
- [[SMT]]: 用于验证 QA 和文档的互补训练信号

## 计算流程

1. 对每个数据点计算 next-token prediction loss 的梯度
2. 使用 Johnson-Lindenstrauss 变换降维
3. 使用 cosine similarity 比较嵌入

## 关键发现

在 SMT 中：
- QA 数据跨域梯度相似度高（≥0.94）
- QA 与文档梯度相似度低（≤0.26）
- 验证 QA 教授行为知识，文档教授事实知识

## 相关概念

- [[Synthetic Mixed Training]]
- [[Vendi Score]]