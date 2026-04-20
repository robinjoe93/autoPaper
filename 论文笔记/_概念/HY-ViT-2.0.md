---
title: HY-ViT 2.0
aliases: [Hunyuan ViT 2.0, HY-ViT]
tags: [vision-encoder, embodied-ai, native-resolution]
created: 2026-04-20
---

# HY-ViT 2.0

## 定义

[[HY-ViT-2.0]] 是腾讯 Hunyuan 团队开发的高效原生分辨率视觉编码器，专为具身 AI 任务设计。它是 HY-ViT 系列的升级版本，支持任意分辨率输入。

## 核心特点

1. **原生分辨率**: 基于 [[NaViT]] 设计，无需 resize 预处理
2. **高效轻量**: 400M 参数，适合边缘部署
3. **知识蒸馏**: 从大型内部 ViT 获得高质量表示
4. **多监督信号**: LLM 语言监督 + 视觉重建监督

## 与 NaViT 的差异

| 特性 | NaViT | HY-ViT 2.0 |
|------|-------|------------|
| 监督 | 自监督 | 语言 + 视觉重建 |
| 知识来源 | 无 | 大型 ViT 蒸馏 |
| 应用 | 通用视觉 | 具身 AI 专用 |

## 离散表示设计

HY-ViT 2.0 大版本生成离散视觉表示：
- **Codebook size**: 2k
- **压缩率**: 每 8×8 patch → 1 个离散码
- **用途**: 监督 MoT 视觉分支的下一码预测

## 在 HY-Embodied-0.5 中的作用

- 视觉编码器，输入为任意分辨率图像/视频
- 输出视觉 Token 进入 MoT 视觉分支
- 全局 CLS 特征监督 Visual Latent Token

## 代表工作

- [[HY-Embodied-0.5]]: 使用 HY-ViT 2.0 的具身 VLM
- HunyuanOCR Technical Report: HY-ViT 系列前身

## 相关概念

- [[NaViT]]: 原生分辨率 ViT 基础设计
- [[Vision-Registers]]: 视觉 Token 设计参考
- [[Mixture-of-Transformers]]: 接收 HY-ViT 输出的架构