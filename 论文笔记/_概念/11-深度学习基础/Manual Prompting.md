---
title: Manual Prompting
tags: [prompting, synthetic-data, data-generation]
created: 2026-04-03
---

# Manual Prompting

## 定义

手动提示方法是指通过人工设计的提示词引导语言模型生成特定内容的合成数据生成方式。

## 特点

- **精细控制**: 可精确指定输出格式和内容
- **高质量**: 专业设计的提示可生成高质量样本
- **适用范围**: 适用于小规模、特定任务

## 局限性

- **规模化困难**: 需大量人工设计提示
- **覆盖受限**: 难以覆盖广泛概念空间
- **一致性差**: 不同提示可能产生不一致结果
- **成本高昂**: 提示工程需要专业知识

## 对比 Simula

[[Simula]] 采用自动化分类法生成：
- 自动生成覆盖空间的节点
- 无需手动设计每个提示
- 可规模化到大规模数据集

## 代表工作

- [[Simula]] (对比方法)
- [[Synthetic Data]]

## 相关概念

- [[In-Context Learning]]
- [[Simula]]
- [[Taxonomy Generation]]