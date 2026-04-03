---
title: Data Curation
tags: [data-selection, dataset-quality, training-data]
created: 2026-04-03
---

# Data Curation

## 定义

数据筛选是指从大规模数据集中选择高质量、高价值样本用于模型训练的过程。

## 核心维度

1. **质量**: 样本是否符合特定要求
2. **多样性**: 样本是否覆盖主要变化因子
3. **复杂度**: 样本的难度或信息量
4. **有用性**: 样本对模型训练的贡献

## 方法

- **基于分类法**: 使用 [[Taxonomy]] 确保覆盖度
- **基于难度**: 选择适当复杂度的样本
- **基于影响力**: 评估样本对训练的贡献

## 在 Simula 中的应用

[[Simula]] 使用分类法进行数据筛选：
- 生成分类法映射概念空间
- 通过 Level Ratio Coverage 评估覆盖度
- 使用 Calibrated Complexity Scoring 评估复杂度

## 代表工作

- [[Simula]]: 分类法驱动数据筛选

## 相关概念

- [[Taxonomy]]
- [[Synthetic Data]]