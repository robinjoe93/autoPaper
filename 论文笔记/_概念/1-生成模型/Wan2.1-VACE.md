---
title: "Wan2.1-VACE"
tags: [video-generation, diffusion-model, video-editing]
created: 2026-04-03
---

# Wan2.1-VACE

## 定义

基于扩散模型的视频生成与编辑模型，支持视频 inpainting 和条件生成。

## 核心特点

- 大规模视频扩散模型 (14B 参数)
- 支持条件控制 (mask, reference)
- 高保真视频合成

## 应用场景

- [[InsertAnywhere]]: 作为视频生成 backbone
- 视频 inpainting
- 视频编辑与合成

## 技术细节

- 通过 [[LoRA]] 微调适配特定任务
- 支持首帧锚定的视频生成
- 50 denoising steps 推理

## 关联概念

- [[Diffusion Model]]
- [[Video Inpainting]]
- [[LoRA]]