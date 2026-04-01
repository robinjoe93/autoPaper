---
type: concept
aliases: [Mauve, Mauve Score]
---

# Mauve

## 定义

通过量化嵌入分布的KL散度前沿曲线下面积，评估真实与合成数据分布相似度的指标。

## 数学形式

$$
\text{Mauve} = \int_0^1 \min\{D_{KL}(P_{\theta}||Q_{\theta}), D_{KL}(Q_{\theta}||P_{\theta})\} d\theta
$$

## 核要点

1. 起源于NLP领域，可扩展到图像数据
2. 使用量化嵌入计算分布距离
3. 有平滑变体Mauve*

## 代表工作

- [[SDQM]]: 作为子指标，与mAP50相关性0.57
- Pillutla et al. (2021): 原始提出

## 相关概念

- [[FID]]
- [[Alpha-Precision]]
- [[Beta-Recall]]
- [[KL Divergence]]