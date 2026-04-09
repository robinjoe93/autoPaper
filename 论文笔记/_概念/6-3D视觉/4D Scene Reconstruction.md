---
title: "4D Scene Reconstruction"
tags: [3d-reconstruction, scene-understanding, video-processing]
created: 2026-04-03
---

# 4D Scene Reconstruction

## 定义

从单目视频重建时间一致的 4D 场景表示（3D 几何 + 时间运动），包括深度、相机姿态、物体运动等。

## 核心思想

- 整合多个预训练视觉模型（深度估计、光流、相机姿态恢复、分割）
- 构建时间一致的几何和运动表示
- 无需专门训练 4D 网络，利用现成模型的组合

## 代表工作

- [[Uni4D]]: 首个系统化的 4D 重建框架
- [[InsertAnywhere]]: 利用 4D 重建生成遮挡感知的 mask

## 技术细节

1. **深度估计**: 从单帧估计稠密深度图
2. **光流估计**: 计算帧间像素对应关系
3. **相机姿态恢复**: 估计每帧的相机位姿
4. **分割**: 区分静态/动态物体

## 应用场景

- 视频对象插入 ([[InsertAnywhere]])
- 视频编辑与特效
- 动态场景理解
- 机器人视觉导航

## 关联概念

- [[3D Reconstruction]]
- [[Optical Flow]]
- [[Camera Pose Estimation]]
- [[Video Segmentation]]