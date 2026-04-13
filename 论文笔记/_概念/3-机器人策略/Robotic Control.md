---
type: concept
aliases: [Robotic Control, Robot Control, 机器人控制]
category: 机器人策略
---

# Robotic Control

## 定义

让机器人执行特定任务的控制问题，包括连续控制（行走、奔跑）和离散操作（抓取、按压）。

## 类型

1. **连续控制**: Walker, Cheetah, Finger 等 locomotion 任务
2. **操作任务**: Assembly, Button-press, Hammer 等机械臂任务
3. **灵巧操作**: Pen, Relocate 等多指手任务

## 视觉控制

Vision-based robotic control:
- 从视觉观测学习策略
- 不依赖状态估计（直接从图像）
- 预训练视觉表征很关键

## 挑战

1. **Domain Gap**: 预训练数据 vs 控制环境
2. **动态性**: 视频流而非静态图像
3. **细粒度**: 需要精确的部件位置信息
4. **数据少**: Imitation learning 演示数据有限

## 代表工作

- [[ORCA]]: Task-adaptive 视觉表征用于控制
- [[VC-1]]: 机器人控制视觉表征 baseline
- [[SCR]]: Stable Diffusion 用于控制

## Benchmarks

- **DeepMind Control**: 连续控制 benchmark
- **MetaWorld**: 机械臂操作 benchmark
- **Adroit**: 灵巧操作 benchmark

## 相关概念

- [[Imitation Learning]]
- [[Behavior Cloning]]
- [[Visual Representation]]