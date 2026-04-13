---
type: concept
aliases: [KL Divergence, Kullback-Leibler Divergence, KL散度]
---

# KL Divergence

## 定义
衡量两个概率分布差异的非对称度量，常用于生成模型的训练目标。

## 数学形式

$$
\text{KL}[q \| p] = \sum_x q(x) \log \frac{q(x)}{p(x)} = -\sum_x q(x) \log p(x) + H(q)
$$

扩散模型变分界：
$$
\text{KL}[q(x) \| p_\theta(x)] \leq \text{KL}[q(z_0, \ldots, z_1 | x) \| p_\theta(z_0, \ldots, z_1)]
$$

## 核心要点
1. 非对称：$\text{KL}[q \| p] \neq \text{KL}[p \| q]$
2. 前向 KL（mode-covering）：倾向于覆盖所有数据模式
3. 反向 KL（mode-seeking）：倾向于聚焦主要模式
4. 扩散模型训练可分解为逐步 KL 最小化

## 代表工作
- [[D-MMD]]: 使用 KL 界分析扩散训练目标
- VAE: 使用 KL 作为正则项

## 相关概念
- [[MMD Loss]]
- [[Teacher Model]]
- [[Discrete Diffusion Models]]