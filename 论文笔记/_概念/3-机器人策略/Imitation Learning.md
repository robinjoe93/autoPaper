---
type: concept
aliases: [模仿学习, Learning from Demonstration, Programming by Demonstration]
---

# Imitation Learning

## 定义

模仿学习是一种机器人学习方法，通过观察和复制人类专家的行为来学习任务策略，无需显式编程或奖励设计。

## 数学形式

给定演示数据集 $\mathcal{D} = \{(o_i, a_i)\}_{i=1}^N$，目标是学习策略 $\pi(a|o)$：

$$
\pi^* = \arg\min_\pi \sum_{i=1}^N \mathcal{L}(\pi(a_i|o_i))
$$

## 核心要点

1. **数据效率**: 相比强化学习，需要更少的数据和训练时间
2. **安全性**: 学习人类演示的安全行为模式
3. **泛化挑战**: 演示数据有限时，难以泛化到新场景
4. **分布漂移**: 推理时的状态分布与训练时不同，可能导致误差累积

## 主要方法

- **Behavior Cloning (BC)**: 直接监督学习
- **Inverse Reinforcement Learning (IRL)**: 从演示推断奖励函数
- **GAIL**: 结合生成对抗网络
- **Diffusion Policy**: 使用扩散模型生成动作

## 代表工作

- [[MimicGen]]: 数据生成系统，扩展模仿学习数据
- [[SoftMimicGen]]: 可变形物体操作的数据生成
- [[Diffusion Policy]]: 扩散模型用于模仿学习

## 相关概念

- [[Sim-to-Real Transfer]]: 仿真到真实迁移
- Behavior Cloning → BC-RNN-GMM