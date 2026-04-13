---
type: concept
aliases: [任务智能体, Task智能体]
---

# Task Agent

## 定义

被实例化用于解决特定任务集的智能体，是 Hyperagents 架构中的执行组件。

## 核心要点

1. **任务执行**: 负责解决给定的目标任务（如代码编辑、论文评审、奖励函数设计）
2. **可被修改**: 其行为和策略可被 [[Meta Agent]] 修改优化
3. **多样化实现**: 可包含任意可计算程序，可选地调用 [[Foundation Models]]、外部工具或学习组件
4. **评估驱动**: 通过在目标任务上的评估分数来衡量性能

## 代表工作

- [[Hyperagents]]: 与 Meta Agent 整合为单一可编辑程序
- [[Darwin Godel Machine]]: 编码智能体同时作为 Task Agent 和 Meta Agent

## 相关概念

- [[Meta Agent]]: 修改 Task Agent 的元级智能体
- [[Foundation Models]]: Task Agent 可调用的基础模型
- [[Agent Systems]]: 智能体系统设计范式