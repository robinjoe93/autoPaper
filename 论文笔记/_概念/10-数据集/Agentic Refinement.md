---
title: Agentic Refinement
tags: [agent, data-quality, iterative-optimization]
created: 2026-04-03
---

# Agentic Refinement

## 定义

智能体细化是指在数据生成过程中，通过迭代式的 Generator-Critic 交互提升样本质量的步骤。

## 核心流程

在 [[Simula]] 中，智能体细化包含：

1. **语义检查**: 验证样本是否符合分类法节点要求
2. **语法检查**: 验证输出格式是否正确
3. **正确性验证**（可选）：对有标准答案的任务使用 [[Double Critic]]
4. **自动修改**: 根据批评解释修改不合格样本

## 设计要点

- **二值判决**: Critic 提供通过/拒绝判断
- **解释反馈**: Critic 解释拒绝原因，指导修改
- **迭代次数**: 可配置细化轮数

## 效果

- 提升样本质量
- 减少标注错误
- 缓解 [[Sycophancy Bias]]

## 代表工作

- [[Simula]]: 智能体细化提升合成数据质量

## 相关概念

- [[Agentic System]]
- [[Double Critic]]
- [[Taxonomy]]