---
type: concept
aliases: [Vision-Language-Action, 视觉语言行动模型]
---

# VLA (Vision-Language-Action)

## 定义

VLA 是一种将视觉、语言理解和机器人动作生成统一在单一模型中的架构范式，使机器人能够理解视觉场景和语言指令，并生成相应的动作序列。

## 核心要点

1. **多模态融合**: 将视觉编码、语言理解和动作生成统一在单一模型
2. **端到端训练**: 从原始输入到动作输出，无需模块化分解
3. **指令跟随**: 通过语言指令指定任务目标和行为
4. **泛化能力**: 利用语言和视觉的预训练知识实现跨任务泛化

## 代表工作

- [[RT-2]]: Google 首个 VLA 模型，将 VLM 输出映射为动作
- [[Pi07]]: Physical Intelligence 的通用机器人策略
- [[OpenVLA]]: 开源 VLA 模型
- [[Octo]]: 开源通用机器人策略

## 相关概念

- [[Vision-Language Model]]: 视觉语言模型基础
- [[Imitation Learning]]: 学习范式
- [[Action Chunking]]: 动作序列生成
- [[Diffusion Policy]]: 另一种动作生成方法