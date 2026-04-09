---
title: "Video Object Insertion"
tags: [video-editing, object-insertion, video-generation]
created: 2026-04-03
---

# Video Object Insertion (VOI)

## 定义

将新对象无缝插入现有视频中，同时保持空间、时间和光照一致性的任务。

## 核心挑战

1. **几何放置**: 确定对象在场景中的正确位置、尺度和姿态
2. **遮挡处理**: 处理动态物体导致的复杂遮挡关系
3. **光照一致性**: 生成符合场景光照的阴影和反射
4. **时间连贯**: 保持跨帧的外观一致性

## 方法分类

### 商业工具
- [[KLing]]: 视频生成工具，支持对象插入
- [[Pika-Pro]]: 视频编辑工具

### 研究方法
- [[GenProp]]: Region-Aware Loss 传播
- [[InsertAnywhere]]: 4D 几何 + 光照感知

## 评估维度

- Subject Consistency (CLIP-I, DINO-I)
- Background Preservation
- Occlusion Integrity
- Lighting Consistency
- Multi-View Consistency

## 关联概念

- [[Video Inpainting]]
- [[4D Scene Reconstruction]]
- [[Diffusion Model]]
- [[ROSE++]]