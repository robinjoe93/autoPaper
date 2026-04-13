---
type: concept
aliases: [自我批判, 元批判]
---

# Meta-Critique

## 定义

一种顺序推理时缩放方法，通过迭代精炼单个验证来识别错误并改进验证质量。

## 核心要点

1. **顺序缩放**: 对单个验证进行迭代精炼
2. **三步流程**: 生成初始验证 → Critique 识别错误 → Merge 精炼
3. **错误类型**: 遗漏的错误、误判的步骤、推理缺陷

## 流程

1. 生成初始验证 $v_{init}$
2. Critique: $\kappa = \text{Critique}(q, s, v_{init})$ 识别错误
3. Merge: $v_{final} = \text{Merge}(v_{init}, \kappa)$ 生成精炼验证

## 代表工作

- [[SPARK]]: 与 Self-Consistency 结合形成 Hybrid 方法
- DeepCritic: 顺序缩放验证方法

## 相关概念

- [[Self-Consistency]]: 并行缩放对比方法
- [[Self-Critique]]: 自我修正的泛化概念
- [[Inference-Time Scaling]]: Meta-Critique 属于顺序缩放