---
type: concept
aliases: [Auxiliary Model, 辅助模型]
---

# Auxiliary Model

## 定义
蒸馏过程中用于跟踪生成器期望分布的模型，与教师模型对比形成对抗训练信号。

## 数学表示

$\hat{x}_\phi(z_s)$: 辅助模型输出

辅助损失：
$$
L_{\text{AUX}}(\phi) = \text{CE}[x | \hat{x}_\phi(z_s)] + \text{CE}[\hat{x}_\theta | \hat{x}_\phi(z_s)]
$$

## 核心要点
1. 辅助模型学习生成器的期望分布
2. 被教师模型正则化，保持接近教师分布
3. 与生成器交替优化，形成稳定的训练过程
4. 固定点：当生成器采样等于教师分布时，辅助模型等于教师

## 代表工作
- [[D-MMD]]: 辅助模型与生成器交替训练
- [[Moment Matching Distillation|MMD]]: 原始 MMD 方法中的关键组件

## 相关概念
- [[Teacher Model]]
- [[Generator]]
- [[MMD Loss]]