---
type: concept
aliases: [VC-1, Visual Cortex 1]
category: 深度学习基础
---

# VC-1

## 定义

FAIR (Meta) 提出的专为机器人控制设计的预训练视觉表征。使用 MAE 在大规模 ego-centric 视频数据上预训练。

## 核心要点

1. **数据**: Ego4D, MVP 数据集（170M 帧）
2. **架构**: ViT-L/16 backbone
3. **训练**: MAE (Masked Autoencoder) 自监督预训练
4. **目标**: 成为机器人控制的 "visual cortex"

## 特点

- **Task-agnostic**: 在所有任务中使用相同的冻结表征
- **Ego-centric**: 训练数据以第一视角为主
- **MAE 预训练**: 掩码重建，适合视觉理解

## 局限性

- 无法适应不同任务的具体需求
- 不同任务上性能差异大（需要手动选择）
- Fine-tuning 会因过拟合导致性能崩溃

## 代表工作

- [[ORCA]]: 作为 baseline 对比，证明 task-adaptive 方法的优越性
- Where are we in the search for an artificial visual cortex for embodied intelligence? (Majumdar et al., 2023)

## 性能对比

在 ORCA 实验中：
- DeepMind Control Mean: 53.1
- MetaWorld Mean: 84.2
- Adroit Mean: 47.3

均低于 ORCA 的 task-adaptive 方法。

## 相关概念

- [[MAE]]
- [[Visual Representation]]
- [[CLIP]]
- [[Imitation Learning]]