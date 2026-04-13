---
type: concept
aliases: [基础模型, FM, Large Language Models, LLM]
---

# Foundation Models

## 定义

在大规模数据上预训练的通用模型，可作为智能体的核心组件提供语言理解、推理和生成能力。

## 核心要点

1. **冻结调用**: 在 Hyperagents 中作为冻结的 FM，智能体通过调用而非修改模型
2. **工具使用**: 可通过外部工具调用扩展能力（bash、文件操作等）
3. **多样化选择**: 不同任务可使用不同 FM（Claude、GPT、o3-mini 等）
4. **推理引擎**: 为 Task Agent 和 Meta Agent 提供推理和生成能力

## 代表工作

- [[Hyperagents]]: 使用 Claude 4.5 Sonnet、o3-mini 等作为基础组件
- [[Darwin Godel Machine]]: 编码智能体使用 Claude 3.5 Sonnet
- Brown et al. (2020): GPT-3 原始论文

## 相关概念

- [[Task Agent]]: 调用 Foundation Models 解决任务
- [[Meta Agent]]: 调用 Foundation Models 生成改进
- [[Agent Systems]]: Foundation Models 是智能体的核心组件