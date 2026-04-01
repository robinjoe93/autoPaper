---
type: concept
aliases: [Alpha-Precision, α-Precision]
---

# Alpha-Precision

## 定义

评估合成数据与真实数据分布稠密区域对齐程度的指标，测量合成样本的保真度。

## 数学形式

$$
\alpha\text{-Precision} = \frac{1}{N} \sum_{i=1}^{N} \mathbb{1}[x_i^{syn} \in \text{support}(P_{real})]
$$

## 核心要点

1. 高Alpha-Precision表示合成数据包含真实、合理的样本
2. 不关注异常值或边缘情况，聚焦分布核心区域
3. 与Beta-Recall互补，共同评估生成质量

## 代表工作

- [[SDQM]]: 作为子指标用于评估合成数据质量

## 相关概念

- [[Beta-Recall]]
- [[Authenticity]]
- [[Mauve]]
- [[FID]]