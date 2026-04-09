---
title: "Uni4D"
tags: [4d-reconstruction, scene-understanding, pretrained-models]
created: 2026-04-03
---

# Uni4D

## 定义

一种 4D 场景建模范式，通过组合多个预训练视觉模型从单目视频构建时间一致的 4D 表示。

## 核心思想

无需训练专门的 4D 网络，而是编排整合：
- 深度估计模型
- 光流模型
- 相机姿态恢复模型
- 分割模型

## 技术架构

1. **深度估计**: 稠密几何重建
2. **光流**: 帧间运动对应
3. **相机姿态**: 相机轨迹估计
4. **分割**: 动态/静态物体区分

## 应用

- [[InsertAnywhere]]: 作为 4D-aware mask generation 的基础
- 动态场景重建
- 视频编辑与合成

## 优势

- 利用预训练模型的强大能力
- 无需专门训练数据
- 模块化设计，易于改进

## 关联概念

- [[4D Scene Reconstruction]]
- [[SEA-RAFT]] (光流)
- [[SAM2]] (分割)