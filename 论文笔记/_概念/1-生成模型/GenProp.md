---
title: "GenProp"
tags: [video-editing, object-propagation, region-aware]
created: 2026-04-03
---

# GenProp

## 定义

一种视频编辑方法，通过编码首帧变化并传播到后续帧实现视频生成。

## 核心方法

- Region-Aware Loss
- 首帧编辑锚定
- 变化传播机制

## 局限性

- 未显式处理可见性变化
- 遮挡场景失败（对象前后关系错误）
- 无法处理对象重新出现

## 对比

与 [[InsertAnywhere]] 相比：
- GenProp: 无 4D 几何理解，遮挡失败
- InsertAnywhere: 4D-aware mask，正确处理遮挡

## 关联概念

- [[Video Object Insertion]]
- [[Video Inpainting]]
- [[InsertAnywhere]]