---
type: concept
aliases: [Consistency Models, CM, 一致性模型]
---

# Consistency Models

## 定义
一类将扩散模型蒸馏为单步生成器的确定性方法，通过学习概率流 ODE 轨迹的一致性映射。

## 核心要点
1. 基于概率流 ODE，学习从任意噪声状态直接映射到数据
2. 使用自身作为目标的一致性约束
3. 支持单步生成，也可多步迭代提高质量
4. 已应用于离散数据（lift to continuous space）

## 代表工作
- Song et al. (2023): 原始一致性模型
- [[D-MMD]]: 批判性思考中提到作为潜在改进方向
- Sahoo et al. (2025): Categorical flow maps for discrete data

## 相关概念
- [[D-MMD]]
- [[Discrete Diffusion Models]]
- [[Moment Matching Distillation|MMD]]