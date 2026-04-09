---
title: "Video Inpainting"
tags: [video-editing, inpainting, diffusion]
created: 2026-04-03
---

# Video Inpainting

## 定义

在视频指定区域填充/编辑内容，保持时间一致性的任务。

## 核心挑战

- 时间一致性（跨帧）
- 内容合理性
- 与周围区域协调

## 方法分类

### 传统方法
- 基于光流的像素传播
- Patch-based 填充

### 深度学习方法
- [[Diffusion Model]] based
- [[Wan2.1-VACE]]
- 首帧锚定方法

## 局限性

- 仅编辑 mask 内区域
- 无法生成局部光照变化
- 自监督训练无光照监督

## 应用场景

- [[Video Object Removal]]
- [[Video Object Insertion]] (逆任务)
- 视频修复与增强

## 关联概念

- [[Image Inpainting]]
- [[Video Object Insertion]]
- [[ROSE++]] (提供光照监督)