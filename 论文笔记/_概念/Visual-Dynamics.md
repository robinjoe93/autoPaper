---
title: "Visual Dynamics"
category: "概念/世界模型"
created: 2026-04-20
---

# Visual Dynamics

## 定义

Visual Dynamics 指视觉观测在时间上的演化规律，即未来观测如何基于当前观测和动作发生变化。

## 在 WAM 中的作用

1. **监督信号**: 提供密集的视觉监督，超越稀疏动作标签
2. **物理约束**: 约束动作预测符合物理可行性
3. **预训练目标**: 学习环境动态先验

## 公式表示

$$
(o_{t+\Delta},\,o_{t+2\Delta},\,\ldots,\,o_{t+K\Delta})\sim g_{\Theta}(\,\cdot\mid o_{t},\,s_{t},\,l,\,c_{t})
$$

## 相关概念

- [[World Model]]
- [[Video Generation]]
- [[World-Action Model]]
- [[Future Prediction]]