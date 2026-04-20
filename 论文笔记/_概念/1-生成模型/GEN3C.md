---
title: "GEN3C"
created: 2026-04-20
tags: [video-diffusion, camera-control, video-generation]
category: 1-生成模型
---

# GEN3C

## 定义

[[GEN3C]] 是一个预训练的相机控制视频扩散模型，能够根据文本提示或单张图像生成具有精确相机运动控制的多视角视频。

## 核心特点

- **相机控制**: 支持精确的相机轨迹控制
- **多视角生成**: 可生成连贯的多视角视频序列
- **大规模预训练**: 在大规模视频数据上预训练
- **隐式3D理解**: 隐含了丰富的3D场景理解能力

## 技术架构

- 基于[[Diffusion Model]]的视频生成架构
- 相机轨迹条件注入机制
- RGB解码器用于生成视频帧

## 在Lyra中的应用

[[Lyra]] 使用GEN3C作为基础模型：
- **冻结预训练组件**: 保持扩散模型和自动编码器冻结
- **新增3DGS解码器**: 与RGB解码器并行
- **自蒸馏监督**: RGB解码器输出监督3DGS解码器训练

## 优势

1. **丰富的3D知识**: 在大规模视频数据上学习了场景几何
2. **多样场景生成**: 可生成各种类型的场景
3. **精确相机控制**: 支持任意的相机轨迹

## 相关概念

- [[Video Diffusion Model]]: 基础技术类别
- [[Lyra]]: 基于GEN3C的3D场景生成方法
- [[Self-Distillation]]: Lyra的训练策略
- [[3DGS Decoder]]: Lyra新增的解码器组件

## 参考文献

Ren et al., "GEN3C: Camera-Controlled Video Generation" (2025)