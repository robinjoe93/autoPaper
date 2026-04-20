---
title: Keyframe-VAE
created: 2026-04-20
tags: [vae, video, reconstruction, world-model]
---

# Keyframe-VAE

## 定义

Keyframe-VAE 是一种专为视频生成和3D重建设计的变分自编码器。不同于标准 Video-VAE 的时空压缩，Keyframe-VAE 仅进行空间压缩，保留关键帧的高保真信息。

## 核心思想

标准 Video-VAE 的时空压缩会导致：
- **运动模糊**: 快速相机运动时质量退化
- **几何扭曲**: 视角变化时出现几何失真
- **重建困难**: 生成的视频难以用于精确3D重建

Keyframe-VAE 通过仅进行空间压缩来解决这些问题。

## 架构设计

$$
\{\mathbf{F}_{i}\}_{i=1}^{1+T_{kf}}\in\mathbb{R}^{1\times\frac{H}{8}\times\frac{W}{8}\times C}
$$

- 对每个关键帧独立使用 causal-padding 图像编码器
- 关键帧数量 $T_{kf} \ll T_{vid}$（远少于标准视频帧数）
- 通过增大采样间隔保持相同的视角覆盖

## 优势

1. **高保真**: 保留高频外观和几何信息
2. **低冗余**: 减少视觉重复帧的冗余
3. **并行化**: 独立编码便于并行处理
4. **重建友好**: 生成的关键帧更适合3D重建

## 应用场景

- [[WorldStereo 2.0]]: 在关键帧潜在空间进行场景生成
- 3D场景重建: 为多视图重建提供高质量输入
- 大视角变化生成: 保持高保真外观一致性

## 与 Video-VAE 对比

| 特性 | Video-VAE | Keyframe-VAE |
|------|-----------|--------------|
| 压缩类型 | 时空压缩 | 空间压缩 |
| 帧质量 | 中等（大视角变化时下降） | 高 |
| 几何保真度 | 低 | 高 |
| 帧冗余 | 高 | 低 |
| 并行性 | 低 | 高 |

## 相关概念

- [[Video Diffusion Model]]: Keyframe-VAE 的应用场景
- [[WorldStereo]]: 使用 Keyframe-VAE 的系统
- [[VAE]]: 基础架构

## 参考文献

- HY-World Team (2026): "HY-World 2.0"