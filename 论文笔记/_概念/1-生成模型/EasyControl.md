---
type: concept
aliases: [EasyControl, 简易控制]
---

# EasyControl

## 定义

一种轻量级可控图像生成框架，通过在扩散模型中添加条件控制模块实现精确的空间和语义控制。

## 核心要点

1. **架构设计**: 在预训练扩散模型（如 SDXL）上添加可训练的控制分支
2. **条件编码**: 支持多种条件类型（姿态图、分割图、深度图等）
3. **训练效率**: 只训练控制模块，保持基础模型冻结
4. **弱条件控制**: 通过 cross-attention 机制注入，无显式空间约束

## 代表工作

- [[PoseDreamer]]: 基于 EasyControl 实现 3D mesh 可控生成，并使用 DPO 对齐提高控制精度

## 相关概念

- [[Controllable Generation]]
- [[Diffusion Model]]
- [[ControlNet]]