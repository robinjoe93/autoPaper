---
type: concept
aliases: [Inception Score, IS]
---

# Inception Score

## 定义

基于预训练Inception v3模型的输出概率，评估生成图像质量和多样性的指标。

## 数学形式

$$
IS = \exp\left(\mathbb{E}_{x \sim P_g}[D_{KL}(p(y|x) || p(y))]\right)
$$

## 核心要点

1. 高IS表示生成图像既多样又可识别为真实物体
2. 不考虑真实样本统计信息
3. 倾向于评估类似Inception训练数据的图像

## 局限性

- 不考虑真实数据分布
- SDQM论文中与mAP50相关性0.59

## 代表工作

- Salimans et al. (2016): 原始提出
- [[SDQM]]: 作为对比基线

## 相关概念

- [[FID]]
- [[Mauve]]