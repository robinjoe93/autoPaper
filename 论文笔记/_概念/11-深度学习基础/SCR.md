---
type: concept
aliases: [SCR, Stable Diffusion for Control and Representation]
category: 深度学习基础
---

# SCR

## 定义

首次将 Stable Diffusion 用于机器人控制和导航任务的视觉表征学习工作。使用 SD 的中间特征作为 task-agnostic 视觉表征。

## 核心要点

1. **Backbone**: Stable Diffusion v1.5
2. **特征提取**: U-Net downsampling blocks + bottleneck
3. **条件**: Null condition（空字符串）
4. **应用**: 机器人控制、导航任务

## 特点

- **Task-agnostic**: 所有任务使用相同的 null condition
- **冻结**: SD 模型参数冻结
- **语义丰富**: 扩散模型特征包含丰富的语义信息

## 局限性

- 使用 null condition，无法实现 task-adaptive
- 在某些任务（如 Cheetah-run）性能下降
- 无法捕获 frame-specific 细粒度细节

## 代表工作

- [[ORCA]]: 作为 baseline，提出 task/visual prompts 替代 null condition
- Pre-trained Text-to-Image Diffusion Models are Versatile Representation Learners for Control (Yang et al., 2024)

## 性能对比

在 ORCA 实验中：
- DeepMind Control Mean: 68.3
- MetaWorld Mean: 90.4
- Adroit Mean: 58.0

## 相关概念

- [[Stable Diffusion]]
- [[Visual Representation]]
- [[VC-1]]
- [[ORCA]]