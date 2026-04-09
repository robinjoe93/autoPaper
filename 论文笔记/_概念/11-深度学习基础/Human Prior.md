---
type: concept
aliases: [人类先验知识, 领域知识注入]
---

# Human Prior

## 定义

从人类研究文献和领域经验中提取的先验知识，包括启发式规则、已知失败模式、设计原则等，用于引导自动化系统的探索方向。

## 数学形式

先验注入：

$$
p_t \sim P(p | S_t, R_t), \quad R_t \leftarrow \text{HumanKnowledge}
$$

**符号说明**:
- $R_t$: 注入的人类先验（来自认知库）
- $S_t$: 历史节点
- 通过条件生成约束探索空间

## 核心要点

1. **来源**: 领域论文、专家经验、失败案例分析
2. **作用**: 避免重复发现已知失败模式，加速有效探索
3. **vs无先验**: 消融显示无认知库时冷启动延迟明显
4. **不限制探索**: 长期演化中系统自身经验占比提升

## 代表工作

- [[ASI-Evolve]]: [[Cognition Base]]即人类先验的实现载体

## 相关概念

- [[Cognition Base]]
- [[Evolutionary Search]]
- Domain Knowledge