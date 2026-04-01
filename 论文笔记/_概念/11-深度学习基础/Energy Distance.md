---
type: concept
aliases: [能量距离, Energy Statistic]
---

# Energy Distance

## 定义

基于能量统计学的两个分布间距离度量，评估多维分布的差异。

## 数学形式

$$
E(X,Y) = 2\mathbb{E}[||X-Y||] - \mathbb{E}[||X-X'||] - \mathbb{E}[||Y-Y'||]
$$

## 核心要点

1. 对分布的均值和方差差异都敏感
2. 可扩展到多维空间
3. 在SDQM中用于比较边界框分布

## 代表工作

- [[SDQM]]: 用于Bounding Box Match子指标

## 相关概念

- [[Wasserstein Distance]]
- [[KL Divergence]]
- [[Anderson-Darling Test]]