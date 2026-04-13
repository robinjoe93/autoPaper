---
type: concept
aliases: [Stable Diffusion, SD, LDM, Latent Diffusion Model]
category: 生成模型
---

# Stable Diffusion

## 定义

Stability AI 发布的文本到图像生成模型，属于 Latent Diffusion Model (LDM)。通过在潜在空间进行扩散过程，实现高效的高质量图像生成。

## 数学形式

扩散过程：

$$
z_t = \sqrt{\bar{\alpha}_t} z_0 + \sqrt{1-\bar{\alpha}_t} \epsilon
$$

训练目标：

$$
\mathcal{L}_{LDM} = \mathbb{E}_{z_0, \epsilon, t, c} \left[ \| \epsilon - \epsilon_\theta(z_t, t, c) \|^2 \right]
$$

**符号说明**:
- $z_0 = \mathcal{E}(x)$: VQGAN encoder 编码的图像 latent
- $z_t$: timestep $t$ 的噪声化 latent
- $\epsilon_\theta$: U-Net denoiser
- $c$: 条件（文本 prompt）

## 核心要点

1. **架构**: VQGAN encoder + U-Net denoiser + VQGAN decoder
2. **条件注入**: Cross-attention 将文本嵌入注入 U-Net
3. **潜在空间**: 降低计算成本，提升生成效率
4. **开源**: 多个版本（v1.4, v1.5, v2.1, SDXL）

## 作为视觉表征的应用

- **SCR**: Stable Diffusion 的中间特征用于控制任务
- **VPD**: 使用 SD 条件增强语义分割
- **DIFT**: SD 特征用于语义对应

## 代表工作

- [[ORCA]]: 使用 SD 1.5 作为 backbone，条件化用于机器人控制
- [[SCR]]: 首次将 SD 用于机器人控制表征学习
- High-Resolution Image Synthesis with Latent Diffusion Models (Rombach et al., 2022)

## 相关概念

- [[Diffusion Model]]
- [[Cross-Attention]]
- [[VQGAN]]
- [[CLIP]] (text encoder)