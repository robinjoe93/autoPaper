---
type: concept
aliases: [Fréchet Inception Distance, FID]
---

# FID

## 定义

通过计算真实与生成图像在Inception网络特征空间中的Fréchet距离，评估生成模型质量的指标。

## 数学形式

$$
FID = ||\mu_r - \mu_g||^2 + Tr(\Sigma_r + \Sigma_g - 2(\Sigma_r \Sigma_g)^{1/2})
$$

## 核心要点

1. 考虑真实样本统计信息，优于[[Inception Score|IS]]
2. 值越小表示生成数据与真实数据分布越接近
3. 广泛用于GAN和扩散模型评估

## 局限性

- 不直接关联下游任务性能
- SDQM论文中与mAP50相关性仅0.45

## 代表工作

- Heusel et al. (2017): 原始提出
- [[SDQM]]: 作为对比基线

## 相关概念

- [[Inception Score|IS]]
- [[Mauve]]
- [[Alpha-Precision]]