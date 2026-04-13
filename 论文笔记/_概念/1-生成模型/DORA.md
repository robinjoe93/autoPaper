---
type: concept
aliases: [DORA-3D]
---

# DORA

## 定义
一种 3D foundation model，采用 VAE + Flow Matching 的 latent-generative 架构，用于高质量 3D 形状生成。

## 架构

- **VAE Encoder $\mathcal{E}$**: 将 3D shape 编码到紧凑 latent space
- **VAE Decoder $\mathcal{D}$**: 从 latent 解码回 spatial domain（occupancy grid）
- **Flow Matching Generator $\mathcal{G}$**: 在 latent space 上训练的生成器

## 核心要点
1. **Latent Space**: 紧凑表示，压缩 3D 数据，支持高效生成
2. **Flow Matching**: 比 Diffusion 更快的采样速度
3. **CFG Support**: 支持 Classifier-Free Guidance，可用于文本引导生成

## Latent Dimension
- Spatial domain: $N^3$（occupancy grid，如 $64^3$）
- Latent domain: $n^3 \times c$（如 $32^3 \times 8$）

## 代表工作
- [[LaS-Comp]]: 使用 DORA 作为 backbone

## 相关概念
- [[3D Foundation Model]]
- [[Flow Matching]]
- [[VAE]]
- [[Classifier-Free Guidance]]