---
type: concept
aliases: [Genesis, Genesis Physics Engine]
---

# Genesis Simulator

## 定义

Genesis (2024) 开发的通用物理仿真平台，用于机器人控制任务的训练和评估。

## 核心要点

1. **四足机器人**: 支持 Go2 quadruped 机器人的仿真
2. **RL训练**: 可用于 PPO 等强化学习算法训练
3. **奖励设计**: Hyperagents 中的奖励函数设计领域使用 Genesis
4. **任务多样**: 支持行走、跳跃等多种运动任务

## 可用环境属性

- `env.commands`: 速度指令
- `env.base_lin_vel`: 基座线速度
- `env.base_pos`: 基座位置
- `env.dof_pos`: 关节位置
- `env.dof_vel`: 关节速度
- `env.actions`: 当前动作

## 代表工作

- [[Hyperagents]]: 机器人奖励设计领域使用
- Genesis (2024): Genesis 原始项目

## 相关概念

- [[Reinforcement Learning]]: Genesis 支持的 RL 训练
- [[Reward Function]]: Genesis 中用于策略训练的目标
- [[Robotics]]: Genesis 的应用领域