---
type: concept
aliases: [Generator, Student, 生成器, 学生模型]
---

# Generator

## 定义
蒸馏过程中待训练的模型，目标是学习教师模型的分布，实现少步高质量生成。

## 数学表示

$\hat{x}_\eta(z_t)$: 生成器输出的软概率向量

生成器损失：
$$
L_{\text{GEN}}(\eta) = \text{CE}[\hat{x}_\eta | \hat{x}_\theta(z_s)] - \text{CE}[\hat{x}_\eta | \hat{x}_\phi(z_s)]
$$

## 核心要点
1. 生成器输出软概率向量而非硬样本
2. 通过对比教师和辅助模型进行更新
3. 在离散域，通过对数概率空间进行矩匹配
4. 因子化输出需要塌缩来捕获相关性

## 代表工作
- [[D-MMD]]: 被蒸馏的少步生成器
- [[Moment Matching Distillation|MMD]]: 学生生成器

## 相关概念
- [[Teacher Model]]
- [[Auxiliary Model]]
- [[MMD Loss]]