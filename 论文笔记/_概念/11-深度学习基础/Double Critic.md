---
title: Double Critic
tags: [evaluation, sycophancy, quality-control]
created: 2026-04-03
---

# Double Critic

## 定义

双重批评是一种评估机制，通过独立查询模型判断答案的"正确性"和"不正确性"，以缓解 [[Sycophancy Bias|恭顺偏差]]。

## 工作原理

传统单批评方法倾向于过度认同生成内容。Double Critic 通过：

1. **正向验证**: 独立查询"答案是否正确？"
2. **负向验证**: 独立查询"答案是否不正确？"
3. **一致性检查**: 只有当正向验证通过且负向验证失败时，才接受答案

## 应用场景

适用于有明确正确性定义的任务：
- 多选题（[[MMLU]], [[GSM8K]]）
- 分类任务
- 事实性问答

## 效果

显著降低标签错误率，提升合成数据质量。

## 代表工作

- [[Simula]]: Double Critic 用于选择题数据生成

## 相关概念

- [[Sycophancy Bias]]
- [[Agentic Refinement]]
- [[Agentic System]]