---
type: concept
aliases: [Improvement Metric, Performance Improvement]
---

# Improvement@k

## 定义

衡量固定元智能体在有限步数内能实现的最大性能提升的度量标准。

## 数学形式

$$
\text{imp@k}(M, A, G, T) = \max_{A' \in \mathcal{A}^{(k)}} \text{Evaluate}(A', T) - \text{Evaluate}(A, T)
$$

其中：
- $M$: 元智能体
- $A$: 初始任务智能体
- $G$: 智能体生成算法
- $T$: 评估任务集
- $k$: 最大修改步数

## 核心要点

1. **最大提升**: 衡量可能的最大改进而非平均
2. **有限预算**: 在固定步数内评估
3. **元级能力**: 反映元智能体生成改进智能体的能力
4. **跨域迁移**: 用于评估元级改进的迁移效果

## 代表工作

- [[Hyperagents]]: 用于评估元智能体的改进能力

## 相关概念

- [[Meta Agent]]: imp@k 衡量的目标对象
- [[Growth Score]]: 用于选择迁移智能体的互补度量