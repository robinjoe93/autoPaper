---
title: "KLing"
tags: [video-generation, commercial-tool, chinese]
created: 2026-04-03
---

# Kling

## 定义

中国快手开发的多模态视频生成工具，支持视频编辑和对象插入。

## 核心功能

- 视频生成与编辑
- 对象插入
- 高质量视频合成

## 局限性

- 对象尺度和姿态推断不准确
- 遮挡处理失败
- 对象保真度较低

## 对比表现

在 [[VOIBench]] 上：
- CLIP-I: 0.6349
- DINO-I: 0.5028
- Motion Smoothness: 0.9940 (最高)
- Multi-View Consistency: 0.5439

## 关联概念

- [[Pika-Pro]] (对比工具)
- [[Video Object Insertion]]
- [[InsertAnywhere]] (超越此工具)