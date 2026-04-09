---
title: "SAM2"
tags: [segmentation, video-segmentation, meta]
created: 2026-04-03
---

# SAM2 (Segment Anything Model 2)

## 定义

Meta 开发的视频分割模型，能够基于首帧提示生成时间一致的分割 mask 序列。

## 核心能力

- 零样本视频分割
- 支持点/框/文本提示
- 时间一致的 mask 序列生成
- 处理遮挡和重新出现

## 应用场景

- [[InsertAnywhere]]: 用于从投影点云提取二值 mask 序列
- 视频编辑与合成
- 视频对象移除
- 视频对象跟踪

## 技术特点

- 基于首帧分割提示
- 传播到后续帧
- 处理复杂遮挡场景

## 关联概念

- [[Video Segmentation]]
- [[SAM]] (图像版本)
- [[4D-aware Mask Generation]]