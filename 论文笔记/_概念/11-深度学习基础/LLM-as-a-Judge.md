---
type: concept
aliases: [LLM评判, 大模型作为裁判, LLM评估]
---

# LLM-as-a-Judge

## 定义

使用大语言模型(LLM)作为评判者来评估生成内容质量、代码复杂度、创新性等难以规则化指标的方法。

## 数学形式

综合评分：

$$
S_{total} = \alpha \cdot S_{metric} + \beta \cdot S_{LLM}
$$

**符号说明**:
- $S_{metric}$: 规则化指标评分（如准确率、loss）
- $S_{LLM}$: LLM评判评分（如代码质量、创新性）
- $\alpha, \beta$: 权重系数

## 核心要点

1. **覆盖维度**: 代码复杂度、效率、创新性、可读性等
2. **评判方式**: 点对点评分、 pairwise比较、排序
3. **提示设计**: 需明确评分标准和尺度
4. **局限性**: 可能存在偏好偏差，需与规则指标结合

## 代表工作

- [[ASI-Evolve]]: 结合LLM Judge评估架构创新性和代码质量
- MT-Bench: 使用LLM评估对话质量

## 相关概念

- [[Evolutionary Agent]]
- [[Neural Architecture Search]]