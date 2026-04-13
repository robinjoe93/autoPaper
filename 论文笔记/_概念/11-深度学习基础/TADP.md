---
type: concept
aliases: [TADP, Text-image Alignment for Diffusion-based Perception]
category: 深度学习基础
---

# TADP

## 定义

使用 VLM 生成的文本描述 + Textual Inversion 学习的风格 token 作为扩散模型条件的方法。用于视觉感知任务（语义分割、深度估计）。

## 核心要点

1. **文本条件**: VLM（如 Gemini）生成的图像描述
2. **风格 token**: 通过 Textual Inversion 学习的特殊 token $S^*$
3. **视觉信息**: 通过单个固定图像捕获的视觉风格
4. **额外优化**: 需要先进行 Textual Inversion

## 与 ORCA 的对比

| 方法 | 视觉信息来源 | 额外优化 | 灵活性 |
|------|-------------|----------|--------|
| TADP | 单个图像的 global style | 需要 Textual Inversion | 有限 |
| ORCA | 每帧 dense features | 不需要（端到端） | 高 |

## 局限性

- 需要额外的 Textual Inversion 步骤
- 视觉信息来自单个图像，无法捕获帧级变化
- 在机器人控制任务中效果有限（minimal gains）

## 代表工作

- [[ORCA]]: TADP 作为 baseline，证明 dense visual prompts 更有效
- Text-image Alignment for Diffusion-based Perception (Huang et al., 2024)

## 性能对比

在 ORCA 实验中：
- DeepMind Control Mean: 70.7
- MetaWorld Mean: 93.1
- Adroit Mean: 57.3

## 相关概念

- [[Textual Inversion]]
- [[Visual Prompt]]
- [[Task Prompt]]