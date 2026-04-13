---
type: concept
aliases: [Video PreTraining]
---

# VPT

## 定义

Video PreTraining，通过观看无标注在线视频学习 Minecraft 动作策略的方法，使用逆向动力学模型标注动作。

## 数学形式

训练流程：
1. IDM 从少量标注数据学习 $p(a_t | o_{t-1}, o_t)$
2. IDM 标注大规模无标注视频
3. BC 训练策略 $\pi(o_t, a_{t-1})$

## 核心要点

1. 使用逆向动力学模型（IDM）标注动作
2. 在 Minecraft 中取得人类水平表现
3. 单游戏智能体，不泛化到其他游戏

## 代表工作

- Baker et al. (2022): VPT 论文

## 相关概念

- [[Behavior Cloning]]
- [[P2P]]
- [[Imitation Learning]]