---
title: "Diffusion Transformer"
category: "架构/生成模型"
created: 2026-04-20
---

# Diffusion Transformer

## 定义

Diffusion Transformer 是结合 Diffusion Model 和 Transformer 架构的生成模型，用 Transformer 替代传统 U-Net 作为去噪网络。

## 核心特点

- **Transformer Backbone**: 使用自注意力机制处理序列数据
- **Flow Matching**: 采用流匹配而非 DDPM 训练
- **多模态支持**: 可处理图像、视频、动作等多种模态

## 代表模型

- Wan 2.2（[[GigaWorld-Policy]] backbone）
- DiT (Diffusion Transformer)
- SD3 (Stable Diffusion 3)

## 相关概念

- [[Diffusion Model]]
- [[Transformer]]
- [[Flow Matching]]
- [[VAE]]