---
type: concept
aliases: [Beta-Recall, β-Recall]
---

# Beta-Recall

## 定义

测量合成数据覆盖真实数据分布程度的指标，评估生成数据的多样性。

## 数学形式

$$
\beta\text{-Recall} = \frac{1}{M} \sum_{j=1}^{M} \mathbb{1}[y_j^{real} \in \text{support}(P_{syn})]
$$

## 核心要点

1. 高Beta-Recall表示合成数据捕获真实数据的多模态分布
2. 避免[[Mode Collapse]]问题
3. 与Alpha-Precision互补，共同评估生成模型质量

## 代表工作

- [[SDQM]]: 作为核心子指标，与mAP50相关性>0.62

## 相关概念

- [[Alpha-Precision]]
- [[Authenticity]]
- [[Mode Collapse]]
- [[Mauve]]