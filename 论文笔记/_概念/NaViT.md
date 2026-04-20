---
title: NaViT (Native Resolution Vision Transformer)
aliases: [NaViT, Patch n' Pack, Native ViT]
tags: [vision-encoder, vit, resolution]
created: 2026-04-20
---

# NaViT (Native Resolution Vision Transformer)

## 定义

[[NaViT]] 是一种支持任意分辨率输入的 Vision Transformer 设计，通过 "Patch n' Pack" 策略将不同分辨率的图像打包处理，无需固定分辨率预处理。

## 核心思想

传统 ViT 需将图像 resize 到固定分辨率（如 224×224），导致：
- 高分辨率图像信息丢失
- 低分辨率图像计算浪费

NaViT 方案：
- 保持原始分辨率
- 不同图像的 patch 序列打包在一起
- 通过 position embedding 标识 patch 来源

## 关键设计

1. **任意分辨率**: 输入图像保持原始尺寸
2. **Patch Packing**: 多张不同尺寸图像打包到同一序列
3. **Factorized Position Embedding**: 分解的位置编码
4. **Dropout by Sequence**: 按序列而非 patch 进行 dropout

## 公式表示

传统 ViT:
$$
\text{Input}: \text{Resize}(I, H_{\text{fixed}} \times W_{\text{fixed}})
$$

NaViT:
$$
\text{Input}: I \text{ at native resolution}
$$

## 代表工作

- **NaViT (Dehghani et al., 2023)**: 原始论文
- [[HY-ViT-2.0]]: HY-Embodied-0.5 的视觉编码器

## 优势

1. **信息保留**: 高分辨率图像不丢失细节
2. **计算效率**: 低分辨率图像不浪费计算
3. **灵活性**: 无需预设分辨率，适配多场景

## 在具身 AI 中的价值

- 机器人观测分辨率变化大（远近物体）
- 精细操作任务需要高分辨率感知
- 边缘设备计算资源有限，低分辨率高效处理

## 相关概念

- [[Vision-Transformer]]: 基础 ViT 架构
- [[HY-ViT-2.0]]: NaViT 的具身 AI 版本