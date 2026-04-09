---
type: concept
aliases: [进化智能体, 进化Agent]
---

# Evolutionary Agent

## 定义

结合LLM智能体能力与进化搜索范式的系统，能够自主生成、评估、改进候选解决方案，在复杂科研任务中实现自动化探索。

## 数学形式

智能体演化过程：

$$
\text{Agent}_{t+1} = \text{Analyze}(\text{Execute}(\text{Design}(\text{Learn}(t))))
$$

**符号说明**:
- Learn: 从历史和认知库获取知识
- Design: LLM生成候选方案
- Execute: 工程执行与评估
- Analyze: 反馈分析与洞察提炼

## 核心要点

1. **智能体组件**: Researcher(设计)、Engineer(执行)、Analyzer(分析)
2. **知识管理**: Cognition Base注入先验，Database积累经验
3. **反馈处理**: 处理多维复杂信号而非单一scalar
4. **任务适用**: 高成本、长周期、弱监督的科研任务

## 代表工作

- [[ASI-Evolve]]: 首个AI-for-AI统一框架
- [[AlphaEvolve]]: 数学与算法发现

## 相关概念

- [[Evolutionary Search]]
- [[Cognition Base]]
- [[LLM-as-a-Judge]]