---
type: concept
aliases: [A-D Test, Anderson-Darling Statistic]
---

# Anderson-Darling Test

## 定义

检验样本是否来自特定分布的非参数统计检验方法，对分布尾部差异敏感。

## 数学形式

$$
A^2 = -n - \sum_{i=1}^{n} \frac{2i-1}{n}[\ln F(X_i) + \ln(1-F(X_{n+1-i}))]
$$

## 核心要点

1. 对分布尾部比[[Kolmogorov-Smirnov Test]]更敏感
2. 非参数方法，无需假设分布类型
3. SDQM中用于Pixel Intensity Match子指标

## 代表工作

- [[SDQM]]: 用于像素强度分布比较

## 相关概念

- [[Kolmogorov-Smirnov Test]]
- [[Energy Distance]]
- [[KL Divergence]]