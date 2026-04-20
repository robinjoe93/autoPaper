---
title: Global-Geometric Memory
created: 2026-04-20
tags: [3d-vision, memory, world-model, geometric-consistency]
---

# Global-Geometric Memory (GGM)

## 定义

Global-Geometric Memory (GGM) 是 [[WorldStereo]] 系列模型中用于维护全局几何一致性的记忆机制。它通过渲染扩展点云作为全局3D先验，使模型能够生成多个几何一致的视频。

## 核心思想

在全景场景中，GGM 使模型内化 $360^{\circ}$ 环境结构，显著提升几何一致性。

关键公式：

$$
\mathbf{P}^{glo}=[\mathbf{P}^{ref},\hat{\mathbf{P}}]\in\mathbb{R}^{(N+\hat{N})\times 3}
$$

其中 $\mathbf{P}^{ref}$ 是参考点云，$\hat{\mathbf{P}}$ 是从 $T_g$ 个新视图随机采样的额外点云。

## 训练策略

### 点云扩展
- 从参考视图和多个新视图构建扩展全局点云
- 新视图点云通过随机采样获得

### 数据增强
为了防止过度拟合到新视图点云：
- 双线性插降采样 50% 深度图（模拟深度 bleeding）
- 10% 样本应用小 Gaussian filter（创建人工 floaters）
- 50% 真实世界数据保留原始单目深度（保留原生 floaters 和噪声）

## 推理过程

推理时使用全景点云 $\mathbf{P}^{pan}$ 作为全局点云，覆盖 $360^{\circ}$ 视点信息作为有效几何引导。

## 作用机制

1. **全局结构**: 维护粗略场景结构的一致性
2. **几何引导**: 通过点云渲染提供几何先验
3. **多轨迹一致**: 确保多个生成视频的几何一致

## 与 SSM++ 配合

- [[GGM]]: 维护全局几何一致性（粗略结构）
- [[Spatial-Stereo Memory]]: 维护局部细节一致性（精细细节）

两者互补，共同确保生成的多视图序列既几何一致又细节丰富。

## 应用场景

- [[WorldStereo]]: 相机引导视频生成
- [[WorldStereo 2.0]]: 关键帧空间的世界扩展
- [[HY-World]]: 多视图世界生成

## 相关概念

- [[Spatial-Stereo Memory]]: 局部细节记忆
- [[Point Cloud]]: GGM 的输入表示
- [[WorldStereo]]: GGM 的应用系统

## 参考文献

- WorldStereo Team (2025): "WorldStereo"
- HY-World Team (2026): "HY-World 2.0"