---
type: concept
aliases: [Stable Diffusion 3.5, SD3.5-Medium, SD3.5-Large]
---

# SD3.5

## 定义

Stability AI 发布的第三代 Stable Diffusion 系列文本到图像生成模型，采用 rectified flow 和 DiT 架构，包含 Medium 和 Large 两个版本。

## 核心要点

1. **Rectified Flow**: 采用直线 velocity field，高效 ODE 采样
2. **DiT 架构**: Diffusion Transformer backbone，取代 U-Net
3. **多版本**: Medium（2B 参数）、Large（8B 参数）
4. **高分辨率**: 支持 512-1024 分辨率生成
5. **文本渲染**: 显著改善文本生成能力

## 代表工作

- [[SOAR]]: 使用 SD3.5-Medium 作为实验 backbone
- Esser et al. (2024): SD3.5 原始论文

## 相关概念

- [[Stable Diffusion]]
- [[Rectified Flow]]
- [[DiT]]
- [[Flow Matching]]