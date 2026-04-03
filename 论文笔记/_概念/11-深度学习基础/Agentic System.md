---
title: Agentic System
tags: [agent, llm, iterative-refinement]
created: 2026-04-03
---

# Agentic System

## 定义

智能体系统是指让大语言模型（LLM）以迭代、自主的方式执行任务的框架，通常包含多轮推理、自我评估和结果细化。

## 核心组件

1. **Generator**: 生成初始输出
2. **Critic**: 评估输出质量，提供反馈
3. **Refiner**: 根据反馈修改输出

## 工作流程

```
初始生成 → 批评评估 → 细化修改 → (迭代) → 最终输出
```

## 在合成数据中的应用

[[Simula]] 使用智能体步骤优化合成数据：
- 批评者检查语义/语法要求
- 双重批评验证正确性
- 自动修改或拒绝不合格样本

## 代表工作

- [[Simula]]: 智能体驱动的数据生成
- [[Agentic Refinement]]

## 相关概念

- [[Double Critic]]
- [[Agentic Refinement]]
- [[In-Context Learning]]