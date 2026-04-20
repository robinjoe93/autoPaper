---
title: Task-Aware Reward
aliases: [任务感知奖励, Heterogeneous Reward Design]
tags: [reinforcement-learning, reward-design, embodied-ai]
created: 2026-04-20
---

# Task-Aware Reward

## 定义

[[Task-Aware-Reward]] 是一种针对具身强化学习的奖励设计策略，根据输出结构设计不同的奖励函数，而非使用单一统一奖励。该设计适配具身任务的异质性。

## 核心公式

$$
r=R_{t}(y,y^{\star})\in[0,1]
$$

其中 $R_t$ 根据任务类型选择不同奖励函数。

## 奖励类型

### 1. Grounding-Based (几何任务)

- **IoU**: Bounding box 重叠度
- **Hungarian IoU**: 多目标匹配
- **Normalized Point Distance**: 点级定位
- **Chamfer Distance**: 点云匹配

### 2. Regression-Based (数值估计)

- **Relative Error Decay**: 相对误差的平滑衰减奖励
- **Distance-based**: 预测值与真值的距离

### 3. Trajectory-Based (轨迹任务)

- **DTW (Dynamic Time Warping)**: 序列相似度
- **Fréchet Distance**: 曲线相似度
- **Endpoint Consistency**: 终点一致性

### 4. Textual-Based (语义任务)

- **Exact Match**: 多选题、判断题
- **LCS (Longest Common Subsequence)**: 序列匹配
- **LLM Judge**: 开放式推理任务

## 设计原则

> **Reward structure should match output structure**

- 密集奖励用于部分正确有意义任务（几何、轨迹）
- 精确匹配用于答案空间明确任务（选择题）
- LLM 评判仅用于真正开放式任务

## 在 HY-Embodied-0.5 中的应用

与 [[GRPO]] 配合：
- 不同任务奖励函数不同，尺度不可比
- GRPO 组内相对标准化统一尺度
- 实现异质任务的统一优化

## 相关概念

- [[GRPO]]: 使用 Task-Aware Reward 的 RL 方法
- [[Embodied-RL]]: 具身强化学习