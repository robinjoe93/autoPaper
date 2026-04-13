---
type: concept
aliases: [DGM, Darwin-Godel-Machine]
---

# Darwin Godel Machine (DGM)

## 定义

一种通过开端式探索实现自我改进的编码智能体系统，通过生成和评估自修改变体形成改进档案。

## 核心要点

1. **编码对齐**: 编码智能体同时作为 Task Agent 和 Meta Agent，依赖任务性能与自我修改技能的对齐假设
2. **开端探索**: 维护智能体档案，成功变体作为 stepping stones 积累
3. **固定元级**: 使用人工设计的固定指令生成机制，限制自我改进潜力
4. **领域限制**: 仅在编码领域有效，难以扩展到其他任务类型

## 代表工作

- [[Hyperagents]]: 扩展 DGM 到任意可计算任务，消除对齐假设
- Zhang et al. (2025): DGM 原始论文

## 相关概念

- [[Hyperagents]]: 直接扩展的基础框架
- [[Godel Machine]]: 自改进AI的理论先驱
- [[Open-endedness]]: DGM 采用的开端式探索策略
- [[Recursive Self-improvement]]: DGM 实现的核心能力