---
title: "Multi-view Fusion"
created: 2026-04-20
tags: [3d-reconstruction, feature-fusion, multi-view]
category: 6-3D视觉
---

# Multi-view Fusion

## 定义

[[Multi-view Fusion]] 是将来自多个视角的图像或特征信息进行融合的技术，用于构建完整的3D场景表示。

## 核心目的

- **信息整合**: 将多个视角的观测整合为完整场景理解
- **互补信息**: 利用不同视角提供的互补几何信息
- **遮挡处理**: 通过多视角观测解决单视角遮挡问题

## 在Lyra中的关键作用

[[Lyra]] 的消融实验显示：
- **去除Multi-view Fusion**: PSNR从24.77降至17.73（大幅下降）
- **关键模块**: 是Lyra架构中最重要的组件之一
- **融合方式**: 使用[[Mamba-2]]进行序列化多视角特征融合

## 技术方法

### 1. 特征级融合

将各视角提取的特征在特征空间融合：
$$
\mathbf{F}_{fused} = \text{Aggregate}(\mathbf{F}_1, \mathbf{F}_2, ..., \mathbf{F}_n)
$$

### 2. 几何级融合

利用相机几何关系进行信息融合：
- Epipolar约束
- 深度一致性
- Visibility判断

### 3. 序列融合（Lyra方法）

使用序列模型处理视角序列：
$$
\mathbf{F}_{fused} = \text{Mamba-2}(\mathbf{F}_{view\_1}, ..., \mathbf{F}_{view\_n})
$$

## 应用场景

- 3D重建
- 新视角合成
- 3D场景生成
- 深度估计
- SLAM

## 挑战

1. **视角一致性**: 确保不同视角信息的一致融合
2. **遮挡处理**: 合理处理被遮挡区域
3. **计算效率**: 高效处理大量视角数据

## 相关概念

- [[3D Gaussian Splatting]]: Lyra的融合目标表示
- [[Mamba-2]]: Lyra使用的融合架构
- [[Lyra]]: 应用Multi-view Fusion的代表性工作
- [[Epipolar Geometry]]: 多视角几何基础

## 参考文献

Lyra论文中的Multi-view Fusion实现