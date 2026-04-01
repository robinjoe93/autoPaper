---
type: concept
aliases: [均方误差损失, Mean Squared Error Loss]
---

# MSE Loss

## 定义

均方误差损失（Mean Squared Error Loss），用于回归任务的常用损失函数，衡量预测值与真实值之间的平方差异。

## 数学形式

$$
\mathcal{L} = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2
$$

## 核心要点

1. 对大误差惩罚更强（平方效应）
2. 常用于数值预测和质量评分任务
3. 梯度计算简单，优化稳定

## 代表工作

- [[UniFilter]]: 用于质量分数预测的训练目标

## 相关概念

- [[Cross-Entropy Loss]]