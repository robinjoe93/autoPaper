---
title: "VOIBench"
tags: [benchmark, video-object-insertion, evaluation]
created: 2026-04-03
---

# VOIBench

## 定义

由 [[InsertAnywhere]] 论文提出的视频对象插入评估基准，包含多样化真实场景。

## 数据组成

- 50 个视频片段
- 室内、室外、自然场景
- 每视频 2 个相关对象
- 总计 100 个测试视频

## 评估维度

1. **Subject Consistency**: CLIP-I, DINO-I
2. **Video Quality**: VBench metrics
   - Background Consistency
   - Subject Consistency
   - Motion Smoothness
   - Imaging Quality
3. **Multi-View Consistency**: 遮挡时的多视角一致性

## 评估方法

- 从每个视频均匀采样 10 帧
- 在对象 mask 区域计算指标
- 用户研究补充定量评估

## 关联概念

- [[Video Object Insertion]]
- [[VBench]]
- [[CLIP-I]]
- [[DINO-I]]