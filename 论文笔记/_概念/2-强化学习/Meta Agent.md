---
type: concept
aliases: [元智能体, Meta智能体]
---

# Meta Agent

## 定义

唯一任务是修改现有智能体并生成新智能体的元级智能体，在 Hyperagents 架构中负责自我改进。

## 核心要点

1. **智能体生成**: 根据历史档案和评估结果提出改进方案
2. **元级修改**: 可修改 Task Agent 的任务解决逻辑，也可修改自身
3. **访问权限**: 可访问整个智能体档案、评估结果和代码库
4. **自引用能力**: 作为可编辑程序的一部分，可以重写自己

## 数学形式

Meta Agent $M$ 的修改操作：

$$
a' = M.\text{Modify}(a, \mathcal{A})
$$

其中 $a$ 是待修改的智能体，$\mathcal{A}$ 是智能体档案。

## 代表工作

- [[Hyperagents]]: Meta Agent 与 Task Agent 整合为 Hyperagent
- [[Darwin Godel Machine]]: 使用固定人工设计的指令生成机制作为 Meta Agent
- [[ADAS]]: 无自我改进能力的 Meta Agent

## 相关概念

- [[Task Agent]]: 被 Meta Agent 修改的执行智能体
- [[Recursive Self-improvement]]: Meta Agent 实现的核心技术路线
- [[Metacognitive Self-modification]]: Meta Agent 对自身的修改能力