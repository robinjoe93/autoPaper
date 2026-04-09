---
type: concept
aliases: [可控生成, 条件生成]
---

# Controllable Generation

## 定义

在生成模型中引入条件控制信号，使生成结果满足特定约束（如姿态、布局、风格、语义等）的图像生成技术。

## 核心要点

1. **条件类型**:
   - 姿态控制：骨骼图、3D mesh
   - 空间控制：分割图、深度图、边缘图
   - 语义控制：文本描述、类别标签

2. **控制方法**:
   - ControlNet：在预训练扩散模型上添加可训练的控制分支
   - T2I-Adapter：轻量级适配器注入控制信号
   - IP-Adapter：图像作为条件的适配器

3. **控制强度**：通过 cross-attention 或额外控制层注入，可调节控制强度

## 代表工作

- [[PoseDreamer]]: 使用 EasyControl 实现 3D 姿态可控生成
- ControlNet (Zhang et al., 2023): 添加控制网络到 Stable Diffusion
- T2I-Adapter (Mou et al., 2023): 轻量级条件适配器

## 相关概念

- [[Diffusion Model]]
- [[Direct Preference Optimization]]
- [[EasyControl]]