---
title: Taxonomy Generation
tags: [taxonomy, llm, concept-mapping]
created: 2026-04-03
---

# Taxonomy Generation

## 定义

分类法生成是指使用模型（如 [[M3|多模态模型]]）自动构建层次化概念分类结构的过程。

## 核心方法

在 [[Simula]] 中，分类法生成采用 **Generator-Critic 交替** 策略：

1. **提议阶段**: 模型 N 次采样生成候选子节点
2. **批评阶段**: 独立调用模型细化提议（添加/删除/合并节点）
3. **规划阶段**（可选）：确保同层节点粒度一致

## 关键公式

$$
\text{M3}(y,\mathcal{S},(d_{0},f_{0}),\cdots,(d_{K},f_{K}))=\left\{\mathcal{T}_{i}\right\}_{i=0}^{K}=\mathcal{T}^{y}
$$

## 优势

- **覆盖边缘情况**: 多次采样提高提议分布
- **减少遗漏**: 批评者补充缺失节点
- **可解释**: 每个节点可追溯到生成过程

## 代表工作

- [[Simula]]: 推理驱动的分类法生成

## 相关概念

- [[Taxonomy]]
- [[M3]]
- [[Generator-Critic]]