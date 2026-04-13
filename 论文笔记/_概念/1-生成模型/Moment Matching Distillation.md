---
type: concept
aliases: [MMD, Moment Matching Distillation, 矩匹配蒸馏]
---

# Moment Matching Distillation

## 定义
一种将扩散模型蒸馏为少步生成器的方法，核心思想是匹配教师模型和生成器的条件期望（第一矩）。

## 数学形式

$$
L_{\text{MMD}}(\eta) = \mathbb{E}_{g_\eta}\left[w(t) \|\mathbb{E}_q[x | z_t] - \mathbb{E}_{g_\eta}[x | z_t]\|^2\right]
$$

交替优化：
$$
L_{\text{MMD}}(\eta) = \mathbb{E}[w(s) \hat{x}_\eta(z_t)^\top \text{sg}(\hat{x}_\phi(z_s) - \hat{x}_\theta(z_s))]
$$

## 核心要点
1. 通过对抗训练让生成器匹配教师分布的条件期望
2. 使用辅助模型跟踪生成器的期望分布
3. 教师和辅助模型的差异驱动生成器更新
4. 当匹配完美时，生成器采样分布等于教师分布

## 代表工作
- [[D-MMD]]: 将 MMD 推广到离散扩散领域
- Salimans et al. (2024): 原始连续域 MMD 方法

## 相关概念
- [[MMD Loss]]
- [[Teacher Model]]
- [[Auxiliary Model]]
- [[Generator]]