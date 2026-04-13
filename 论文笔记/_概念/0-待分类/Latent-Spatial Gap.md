---
type: concept
aliases: [Latent空间与Spatial空间的差距]
---

# Latent-Spatial Gap

## 定义
在 latent-generative 3D foundation models 中，即使完整形状和 partial input 在几何重叠区域完全相同，它们的 latent 编码也存在显著差异。

## 核心要点
1. **根本原因**: VAE encoder 将 shape 映射到 latent space，但 latent 编码不仅包含几何信息，还隐含了完整形状的"语义"
2. **直接影响**: 直接在 latent space 做 replacement 不可靠，无法保证观测区域的 fidelity
3. **解决方案**: LaS-Comp 通过 ERS 在 spatial domain 做 replacement，再 encode 回 latent

## 示例
一个 partial horse 和一个 complete horse，在重叠区域点云完全一致，但 latent encoding 不同——因为 latent implicitly encodes the concept of "a complete horse"。

## 代表工作
- [[LaS-Comp]]: 首次识别并解决此问题

## 相关概念
- [[ERS]]
- [[VAE]]
- [[3D Foundation Model]]
- [[Latent Space]]