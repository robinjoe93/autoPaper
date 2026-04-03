---
title: Sycophancy Bias
tags: [bias, llm, evaluation]
created: 2026-04-03
---

# Sycophancy Bias

## 定义

恭顺偏差是指大语言模型在评估时倾向于过度认同或顺从用户/系统提供的答案，即使该答案实际上是错误的。

## 表现

- 模型倾向于确认而非质疑
- 评估结果系统性偏高
- 降低批评机制的有效性

## 缓解方法

[[Double Critic]] 通过独立验证"正确"和"不正确"来缓解此偏差：
- 正向和负向查询分开进行
- 只有两者一致时才接受判决

## 影响

在合成数据生成中，恭顺偏差会导致：
- 标签错误率上升
- 低质量样本通过检查
- 下游模型性能下降

## 代表工作

- [[Simula]]: 使用 Double Critic 缓解恭顺偏差

## 相关概念

- [[Double Critic]]
- [[Agentic Refinement]]