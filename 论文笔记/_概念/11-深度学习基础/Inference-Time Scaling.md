---
type: concept
aliases: [推理时计算缩放, Test-Time Compute Scaling]
---

# Inference-Time Scaling

## 定义

一种通过在推理阶段分配额外计算资源来提高 LLM 推理能力的方法，而非增加模型参数或训练时间。

## 核心要点

1. **两大类别**: 并行缩放（Self-Consistency）和顺序缩放（Meta-Critique）
2. **优于参数缩放**: 最优推理时计算可超越简单增加参数
3. **无需 Ground Truth**: 可在无参考答案情况下改进推理

## 代表工作

- [[SPARK]]: 利用推理时缩放生成 PRM 训练数据
- Snell et al. (2025): 推理时计算最优缩放研究
- Brown et al. (2024): Large Language Monkeys

## 相关概念

- [[Self-Consistency]]: 并行缩放代表方法
- [[Meta-Critique]]: 顺序缩放代表方法
- [[Process Reward Model|PRM]]: 推理时缩放可用于生成 PRM 数据