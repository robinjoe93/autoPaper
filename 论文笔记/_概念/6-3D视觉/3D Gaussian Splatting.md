---
title: "3D Gaussian Splatting"
created: 2026-04-20
tags: [3d-representation, real-time-rendering, novel-view-synthesis]
category: 6-3D视觉
---

# 3D Gaussian Splatting

## 定义

[[3D Gaussian Splatting]] (3DGS) 是一种用于3D场景表示和实时渲染的技术，通过一组3D Gaussian函数来表示场景，每个Gaussian包含位置、颜色、透明度、尺度、旋转等属性。

## 核心思想

- **显式表示**: 使用显式的3D Gaussian集合而非隐式神经网络
- **实时渲染**: 通过可微分的Gaussian splatting实现高效渲染
- **可微分优化**: 通过梯度下降优化Gaussian参数

## 数学表示

每个3D Gaussian由以下参数定义：
- 位置 $\mathbf{p} \in \mathbb{R}^3$
- 颜色 $c$（通过球谐函数表示）
- 透明度 $o \in [0, 1]$
- 尺度 $\mathbf{s} \in \mathbb{R}^3$
- 旋转 $\mathbf{R} \in \mathbb{R}^4$（四元数）

Gaussian的协方差矩阵：
$$
\Sigma = \mathbf{R}\mathbf{S}\mathbf{S}^T\mathbf{R}^T
$$

## 渲染过程

1. 将3D Gaussians投影到2D屏幕空间
2. 按深度排序Gaussians
3. 使用alpha blending进行颜色混合

## 代表工作

- **原始论文**: Gaussian Splatting for Real-Time Radiance Field Rendering (SIGGRAPH 2023)
- **动态扩展**: 4D Gaussian Splatting for Real-Time Dynamic Scene Rendering
- **生成式应用**: [[Lyra]] - 使用自蒸馏框架生成3DGS场景

## 应用场景

- 新视角合成
- 3D场景重建
- 虚拟环境生成
- 机器人仿真场景构建
- AR/VR内容创建

## 优势

1. **实时渲染**: 比NeRF等隐式方法快100-1000倍
2. **高质量**: 可达到与NeRF相当的视觉质量
3. **可编辑**: 显式表示便于场景编辑和修改
4. **内存效率**: 原始方法内存占用较大，Latent 3DGS改进了这一问题

## 相关概念

- [[NeRF]]: 隐式神经辐射场表示
- [[Latent 3DGS]]: 在潜空间表示3D Gaussians
- [[Multi-view Fusion]]: 多视角特征融合用于Gaussian生成
- [[Lyra]]: 使用自蒸馏生成3DGS场景