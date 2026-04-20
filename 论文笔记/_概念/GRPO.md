---
title: GRPO (Group Relative Policy Optimization)
aliases: [GRPO, DeepSeekMath GRPO]
tags: [reinforcement-learning, llm-training, policy-optimization]
created: 2026-04-20
---

# GRPO (Group Relative Policy Optimization)

## 定义

[[GRPO]] 是一种强化学习优化方法，由 DeepSeek-Math 提出，通过在采样组内相对标准化奖励来计算优势函数，适合处理任务异质性强的场景。

## 核心公式

### 优势函数计算

$$
A_{i}=\frac{r_{i}-\mu(\mathbf{r})}{\sigma(\mathbf{r})},\qquad\mathbf{r}=\{r_{1},\dots,r_{G}\}
$$

### 策略损失

$$
\mathcal{L}_{\mathrm{RL}}(x)=-\frac{1}{\sum_{i=1}^{G}|y_{i}|}\sum_{i=1}^{G}\sum_{t=1}^{|y_{i}|}\min\!\Big(\rho_{i,t}A_{i},\,\mathrm{clip}(\rho_{i,t},1-\epsilon_{\mathrm{low}},1+\epsilon_{\mathrm{high}})A_{i}\Big)
$$

## 与 PPO 的区别

| 特性 | PPO | GRPO |
|------|-----|------|
| 优势估计 | 单样本估计 | 组内相对标准化 |
| 适用场景 | 同质任务 | 异质任务 |
| 值函数 | 需要 Critic | 无需 Critic |

## 优势

1. **无需 Critic**: 简化训练流程
2. **处理异质性**: 不同任务奖励不可直接比较时，组内相对标准化有效
3. **稳定性**: 组内均值/方差作为基准，减少奖励尺度影响

## 在具身 RL 中的应用

HY-Embodied-0.5 使用 GRPO 处理具身任务的异质性：
- Grounding 任务 → IoU 基奖励
- Trajectory 任务 → DTW/Fréchet 基奖励
- 推理任务 → LLM 评判奖励

不同奖励尺度通过组内相对标准化统一。

## 相关概念

- [[PPO]]: Proximal Policy Optimization
- [[DeepSeek-R1]]: 使用 GRPO 的代表性工作
- [[Task-Aware Reward]]: 与 GRPO 配合的奖励设计