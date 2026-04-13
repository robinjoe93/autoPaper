---
type: concept
aliases: [Teacher Model, Teacher, 教师模型]
---

# Teacher Model

## 定义
蒸馏过程中的原始模型，提供训练信号指导学生模型学习。在扩散蒸馏中，通常是训练好的多步扩散模型。

## 数学表示

$\hat{x}_\theta(z_t)$: 教师模型输出，近似 $\mathbb{E}_q[x | z_t]$

## 核心要点
1. 教师模型参数固定，不参与蒸馏训练
2. 提供条件期望的参考标准
3. 在 MMD 中，教师与辅助模型的差异驱动生成器更新
4. 奇特现象：学生模型可超越教师（因蒸馏具有 mode-seeking 特性）

## 代表工作
- [[D-MMD]]: 教师模型为 Masked/Uniform Diffusion 模型
- [[Moment Matching Distillation|MMD]]: 教师为连续扩散模型

## 相关概念
- [[Auxiliary Model]]
- [[Generator]]
- [[Moment Matching Distillation|MMD]]