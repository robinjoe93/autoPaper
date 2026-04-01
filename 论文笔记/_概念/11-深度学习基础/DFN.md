---
type: concept
aliases: [Data Filtering Network]
---

# DFN

## 定义

Data Filtering Network，Apple 提出的高质量数据过滤模型，通过持续预训练 CLIP 学习评估数据质量。

## 核心要点

1. 基于 CLIP-large 架构的质量分类器
2. 在高质量数据上持续预训练
3. 仅支持 image-text caption 数据过滤

## 代表工作

- [[UniFilter]]: 主要对比基线，UniFilter 突破其 interleaved 数据限制

## 相关概念

- [[CLIPScore]]
- [[MLMFilter]]