---
title: "Diffusion Model"
tags: [generative-model, diffusion, video-generation]
created: 2026-04-03
---

# Diffusion Model

## 定义

一类生成模型，通过逐步去噪从噪声分布生成目标数据。

## 核心原理

1. **前向过程**: 逐步添加噪声
2. **反向过程**: 学习去噪，生成数据

## 视频生成应用

- [[Wan2.1-VACE]]: 视频条件生成
- [[InsertAnywhere]]: 视频对象插入
- [[Sora]], [[KLing]], [[Pika-Pro]]

## 条件控制

- Mask 条件 (inpainting)
- Reference image 条件
- Text 条件

## 关联概念

- [[Video Inpainting]]
- [[LoRA]] (高效微调)
- [[Video Object Insertion]]