---
title: "Pika-Pro"
tags: [video-generation, commercial-tool]
created: 2026-04-03
---

# Pika-Pro

## 定义

Pika 开发的商业视频编辑工具，支持视频对象插入等高级编辑功能。

## 核心功能

- 视频编辑
- 对象插入
- 多模态控制

## 局限性

在 [[VOIBench]] 评估中表现最弱：
- CLIP-I: 0.4940 (最低)
- DINO-I: 0.3856 (最低)
- Occlusion Integrity: 0% (完全失败)
- 对象保真度严重不足

## 对比表现

- 对象尺度错误（胡椒瓶过大）
- 颜色偏差严重
- 遮挡处理完全失败

## 关联概念

- [[KLing]] (对比工具)
- [[Video Object Insertion]]
- [[InsertAnywhere]] (显著超越)