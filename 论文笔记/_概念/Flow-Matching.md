---
title: "Flow Matching"
category: "方法/训练框架"
created: 2026-04-20
---

# Flow Matching

## 定义

Flow Matching 是一种训练生成模型的方法，通过匹配从噪声到数据的概率路径（流）来学习生成过程。

## 核心公式

**噪声插值**:
$$
x^{(s)}=(1-s)\epsilon+sx
$$

**目标速度场**:
$$
\dot{x}^{(s)}=x-\epsilon
$$

**训练目标**:
$$
\mathcal{L}=\mathbb{E}\left[\|v_\theta(x^{(s)}, s) - \dot{x}^{(s)}\|^2\right]
$$

## 与 DDPM 的区别

| 特性 | Flow Matching | DDPM |
|------|---------------|------|
| 训练目标 | 速度场 | 噪声预测 |
| 采样路径 | 直线路径 | 随机路径 |
| 采样步数 | 更灵活 | 固定 |

## 代表工作

- [[GigaWorld-Policy]]: 用于动作和视频生成
- Rectified Flow
- Stable Diffusion 3

## 相关概念

- [[Diffusion Model]]
- [[Diffusion Transformer]]
- [[ODE Sampling]]