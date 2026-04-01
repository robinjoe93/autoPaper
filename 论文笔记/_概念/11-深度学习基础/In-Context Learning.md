---
type: concept
aliases: [上下文学习, Few-shot Learning]
---

# In-Context Learning

## 定义

上下文学习（In-Context Learning），LLM 通过 few-shot demonstration 示例理解任务并完成推理，无需参数更新。

## 数学形式

$$
P(y | x, \text{demos}) = \text{LLM}([\text{demo}_1, ..., \text{demo}_k, x, y])
$$

## 核心要点

1. 通过 demonstration 提供任务信息
2. 无需 gradient update，推理时适应
3. interleaved 数据训练显著提升 ICL 能力

## 代表工作

- [[GPT-3]]: 首次展示大规模 ICL 能力
- [[UniFilter]]: Interleaved 数据筛选提升 MLLM ICL 性能

## 相关概念

- [[MLLM]]
- [[OBELICS]]