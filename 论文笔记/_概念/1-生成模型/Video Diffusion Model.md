---
title: "Video Diffusion Model"
created: 2026-04-20
tags: [diffusion-model, video-generation, temporal-modeling]
category: 1-生成模型
---

# Video Diffusion Model

## 定义

[[Video Diffusion Model]] 是一类基于扩散过程的视频生成模型，通过逐步去噪的方式生成连贯的视频序列。

## 核心原理

扩散过程：
- **前向过程**: 向视频数据逐步添加噪声
- **反向过程**: 学习去噪过程，从噪声生成视频

$$
q(\mathbf{x}_t|\mathbf{x}_{t-1}) = \mathcal{N}(\mathbf{x}_t; \sqrt{1-\beta_t}\mathbf{x}_{t-1}, \beta_t\mathbf{I})
$$

## 关键技术组件

1. **时序建模**: 处理帧间时间依赖关系
2. **3D U-Net**: 扩展2D U-Net到时序维度
3. **条件注入**: 支持文本、图像、相机轨迹等条件

## 代表模型

- [[GEN3C]]: 相机控制视频扩散模型
- Stable Video Diffusion
- Video LDM
- AnimateDiff

## 在Lyra中的作用

[[Lyra]] 利用视频扩散模型的隐式3D知识：
- 视频模型学习了场景的空间结构
- 通过多视角视频生成提供3D监督信号
- 自蒸馏框架提取隐式3D知识到显式3DGS

## 相机控制视频扩散

特殊类型的视频扩散模型：
- 输入相机轨迹作为条件
- 生成相机运动对应的多视角视频
- 隐含对场景几何的理解

## 相关概念

- [[Diffusion Model]]: 基础扩散模型概念
- [[GEN3C]]: Lyra使用的具体模型
- [[Self-Distillation]]: 利用视频模型的训练策略
- [[3DGS Decoder]]: 从视频模型蒸馏的组件

## 应用场景

- 视频内容创作
- 多视角视频合成
- 3D场景生成（如Lyra）
- 动态场景重建