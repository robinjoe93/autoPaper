---
type: concept
aliases: [MMD Loss, 矩匹配损失]
---

# MMD Loss

## 定义
Moment Matching Distillation 中的核心损失函数，用于衡量生成器与教师模型分布的差异。

## 数学形式

连续域：
$$
L_{\text{MMD}}(\eta) = \mathbb{E}_{g_\eta}\left[w(s) \hat{x}_\eta(z_t)^\top (\hat{x}_\phi(z_s) - \hat{x}_\theta(z_s))\right]
$$

离散域（D-MMD）：
$$
L_{\text{GEN}}(\eta) = \text{CE}[\hat{x}_\eta | \hat{x}_\theta(z_s)] - \text{CE}[\hat{x}_\eta | \hat{x}_\phi(z_s)]
$$

## 核心要点
1. 通过对比教师和辅助模型的输出驱动生成器学习
2. 在离散域，使用对数概率空间而非直接输出空间
3. 权重函数 $w(s)$ 平衡不同时间步的重要性
4. 辅助模型损失 $L_{\text{AUX}}$ 同时优化辅助模型

## 代表工作
- [[D-MMD]]: 离散版本的 MMD Loss 实现

## 相关概念
- [[Moment Matching Distillation|MMD]]
- [[Auxiliary Model]]
- [[KL Divergence]]