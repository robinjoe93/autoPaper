---
title: "Object Placement"
tags: [3d-placement, user-control, object-insertion]
created: 2026-04-03
---

# Object Placement

## 定义

在 4D 场景中确定插入对象的位置、尺度和姿态的过程，需要用户控制和几何理解结合。

## 核心公式

$$
y^{\prime}_{j}=s_{\text{obj}}\,R_{\text{obj}}\,y_{j}+t_{\text{obj}}
$$

将对象点云从局部坐标变换到场景坐标。

## 参数说明

- $s_{\text{obj}}$: 用户控制的尺度因子
- $R_{\text{obj}} \in SO(3)$: 用户控制的旋转矩阵
- $t_{\text{obj}} \in \mathbb{R}^{3}$: 用户控制的平移向量

## 用户交互

- 通过 GUI 在重建的 4D 场景中导航
- 在首帧精确放置对象
- 控制尺度、姿态、位置

## 应用场景

- [[InsertAnywhere]]: 用户控制的对象放置
- 虚拟产品放置
- 视频特效制作

## 关联概念

- [[4D Scene Reconstruction]]
- [[4D-aware Mask Generation]]
- [[Camera Reprojection]]