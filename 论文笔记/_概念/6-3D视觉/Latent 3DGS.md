---
title: "Latent 3DGS"
created: 2026-04-20
tags: [3d-representation, memory-efficient, latent-space]
category: 6-3D视觉
---

# Latent 3DGS

## 定义

[[Latent 3DGS]] 是在潜空间表示3D Gaussian Splatting的技术，通过在压缩的潜空间中表示Gaussian参数，大幅降低内存需求。

## 核心思想

- **潜空间表示**: 不直接存储高维Gaussian参数，而是在压缩的潜空间中表示
- **解码渲染**: 通过解码器将潜表示转换为可渲染的Gaussian参数
- **内存效率**: 显著减少显存占用，避免OOM问题

## 与原始3DGS的区别

| 特性 | 原始3DGS | Latent 3DGS |
|------|----------|-------------|
| 参数存储 | 直接存储高维参数 | 潜空间压缩表示 |
| 内存占用 | 较大 | 显著降低 |
| 渲染方式 | 直接splatting | 解码后splatting |
| 可编辑性 | 高 | 受解码器限制 |

## 在Lyra中的重要性

[[Lyra]] 的消融实验表明：
- 去除Latent 3DGS会导致OOM（Out of Memory）
- 对内存效率至关重要
- 支持大规模3D场景生成

## 技术架构

1. **潜表示编码器**: 将Gaussian参数压缩到潜空间
2. **潜表示解码器**: 从潜表示恢复Gaussian参数
3. **可微分渲染**: 支持梯度反向传播

## 数学表示

潜表示 $\mathbf{z}$ 与Gaussian参数 $\mathbf{g}$ 的关系：
$$
\mathbf{g} = \text{Decoder}(\mathbf{z})
$$

其中 $\mathbf{z} \in \mathbb{R}^{d_z}$，维度远小于原始Gaussian参数维度

## 优势

1. **内存效率**: 避免OOM，支持更大规模场景
2. **训练效率**: 潜空间优化更稳定
3. **可扩展性**: 支持动态场景扩展

## 相关概念

- [[3D Gaussian Splatting]]: 原始技术
- [[Lyra]]: 应用Latent 3DGS的方法
- [[Mamba-2]]: Lyra中用于潜表示处理的架构

## 参考文献

Lyra论文中的Latent 3DGS实现