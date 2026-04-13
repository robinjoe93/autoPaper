---
type: concept
aliases: [元认知自我修改, Metacognitive Modification]
---

# Metacognitive Self-modification

## 定义

Hyperagents 的核心能力：智能体不仅能改进任务执行行为，还能改进生成未来改进的机制本身。

## 核心要点

1. **双重改进**: 同时改进 Task Agent（任务行为）和 Meta Agent（改进机制）
2. **机制可编辑**: 自我改进程序本身是可编辑代码的一部分
3. **突破限制**: 打破固定人工设计元级机制的上限
4. **跨域适用**: 不依赖任务与自我修改技能的对齐假设

## 代表工作

- [[Hyperagents]]: 元认知自我修改的核心实现框架
- [[Self-referential Learning]]: 元认知自我修改的理论基础

## 发现的改进案例

Hyperagents 自动发现的元级改进包括：
- **Persistent Memory**: 创建 MemoryTool 存储跨代知识
- **Performance Tracker**: 记录各代性能用于数据驱动决策
- **Prompt Template Systems**: 抽象提示词为可复用模板
- **UCB-based Selection**: 自动发现置信上限父代选择策略

## 相关概念

- [[Meta Agent]]: 元认知自我修改的目标组件
- [[Self-referential Learning]]: 元认知自我修改的理论形式
- [[Recursive Self-improvement]]: 元认知自我修改实现的终极目标