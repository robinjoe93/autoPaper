---
title: "3DGS Decoder"
created: 2026-04-20
tags: [3d-generation, decoder, gaussian-splatting]
category: 6-3D视觉
---

# 3DGS Decoder

## 定义

[[3DGS Decoder]] 是一种神经网络模块，用于从编码特征生成[[3D Gaussian Splatting]]表示，是Lyra框架中的核心组件。

## 核心功能

- **输入**: 扩散模型编码的潜特征
- **输出**: 3D Gaussian参数集合
- **训练**: 通过自蒸馏由RGB解码器监督

## 在Lyra中的设计

### 架构组成

1. **潜空间处理**: 在压缩潜空间中操作
2. **[[Mamba-2]]融合**: 处理多视角特征序列
3. **时间条件注入**: 支持动态场景生成
4. **参数生成**: 输出Gaussian的位置、颜色、透明度等

### 训练策略

- **自蒸馏**: RGB解码器输出作为监督
- **冻结其他组件**: 仅训练3DGS解码器
- **多损失函数**: MSE + LPIPS + Depth + Opacity

## 数学表示

解码过程：
$$
\mathbf{G} = \text{Decoder}_{3DGS}(\mathbf{z}, t)
$$

其中：
- $\mathbf{z}$: 潜特征
- $t$: 时间条件（动态场景）
- $\mathbf{G}$: 3D Gaussian参数集

## 推理时使用

训练完成后：
- 直接使用3DGS解码器
- 无需RGB解码器
- 输出可直接渲染的3DGS场景

## 与RGB解码器关系

| 组件 | 作用 | 训练状态 |
|------|------|----------|
| RGB解码器 | 教师，提供监督 | 冻结 |
| 3DGS解码器 | 学生，学习生成 | 训练 |

## 优势

1. **显式表示**: 输出可直接渲染的3DGS
2. **实时渲染**: 支持实时场景渲染
3. **动态扩展**: 通过时间条件支持动态场景
4. **内存效率**: 使用Latent 3DGS避免OOM

## 相关概念

- [[Lyra]]: 3DGS解码器的应用框架
- [[Self-Distillation]]: 3DGS解码器的训练策略
- [[3D Gaussian Splatting]]: 输出表示形式
- [[Latent 3DGS]]: 高效Gaussian表示
- [[GEN3C]]: 基础视频扩散模型