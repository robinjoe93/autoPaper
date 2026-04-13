---
type: concept
aliases: [高质量网页数据集]
---

# FineWeb

## 定义

FineWeb 是一个大规模高质量网页数据集，用于语言模型预训练和 replay 数据源。

## 核心要点

1. **高质量**: 经过严格清洗和过滤的网页文本
2. **大规模**: 包含大量 tokens，适合预训练
3. **Replay 用途**: 在 continued pretraining 中用于缓解遗忘
4. **开源**: 公开可获取

## 代表工作

- Penedo et al. (2024): 提出 FineWeb 数据集
- [[SMT]]: 作为 replay 数据源（10% 混合比例）

## 用途

- 预训练数据源
- Continued pretraining replay
- 遗忘缓解

## 相关概念

- [[Synthetic Mixed Training]]
- [[Synthetic Data]]